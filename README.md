# dbeaver-license
Generate activation licenses for all DBeaver editions. Currently supports version `23.0` and newer versions.

## Prerequisites
- Oracle JDK 11 or higher
- The required JAR dependencies are already included in the `lib/` directory

## Building and Running

### Step 1: Compile the Application
```bash
javac -cp "lib/*" src/com/github/oiltea/LMMain.java -d .
```

### Step 2: Run the Application
```bash
# Windows
java -cp ".;lib/*" com.github.oiltea.LMMain

# Linux/Mac
java -cp ".:lib/*" com.github.oiltea.LMMain
```

### Step 3: Generate License Keys
When prompted, enter the following information:
- License type: `UE` (for Ultimate), `EE` (for Enterprise), or `LITE` (for Lite)
- Product ID: Accept default or customize
- Product version: Accept default or customize
- Owner details: ID, Company, Name, Email

The program will generate:
- PUBLIC KEY
- PRIVATE KEY  
- LICENSE

Save these to a file (e.g., `keys.txt`) for the activation process.

## Activating DBeaver

### Step 1: Backup and Modify the Plugin JAR

1. Navigate to your DBeaver installation plugins directory
2. Find the appropriate plugin JAR:
   - **DBeaver Lite**: `com.dbeaver.app.lite_xx.x.x.xxxxxxxxxxxx.jar`
   - **DBeaver Enterprise**: `com.dbeaver.app.enterprise_xx.x.x.xxxxxxxxxxxx.jar`
   - **DBeaver Ultimate**: `com.dbeaver.app.ultimate_xx.x.x.xxxxxxxxxxxx.jar`

3. Create a backup of the JAR file
4. Extract and replace the public key:

```bash
# Example for DBeaver Ultimate on Windows
cd C:\tmp
jar xf "C:\Users\User\AppData\Local\DBeaverUltimate\plugins\com.dbeaver.app.ultimate_25.1.0.202506090822.jar" keys/dbeaver-ue-public.key

# Replace the key content with your generated PUBLIC KEY
# Then update the JAR
jar uf "C:\Users\User\AppData\Local\DBeaverUltimate\plugins\com.dbeaver.app.ultimate_25.1.0.202506090822.jar" keys/dbeaver-ue-public.key
```

The public key files are located at:
- **Lite**: `keys/dbeaver-le-public.key`
- **Enterprise**: `keys/dbeaver-ee-public.key`
- **Ultimate**: `keys/dbeaver-ue-public.key`

### Step 2: Create License Directory and Add Keys

Create the `.jkiss-lm` directory in your user home:

```bash
# Windows
mkdir C:\Users\%USERNAME%\.jkiss-lm

# Linux/Mac
mkdir ~/.jkiss-lm
```

Create two files in the `.jkiss-lm` directory:
- `public-key.txt` - Paste your generated PUBLIC KEY (without the "--- PUBLIC KEY ---" headers)
- `private-key.txt` - Paste your generated PRIVATE KEY (without the "--- PRIVATE KEY ---" headers)

### Step 3: Import License

1. Launch DBeaver
2. Go to Help → Import License
3. Paste the generated LICENSE content
4. Click OK to activate

## Preventing Online Validation

To prevent license validation issues, add the following entry to your hosts file:

```
127.0.0.1 dbeaver.com
```

**Hosts file location:**
- Windows: `C:\Windows\System32\drivers\etc\hosts`
- Linux/Mac: `/etc/hosts`

Note: You need administrator/root privileges to edit the hosts file.

## Complete Example Walkthrough

Here's a complete example of activating DBeaver Ultimate on Windows:

1. **Compile and run the generator:**
```bash
javac -cp "lib/*" src/com/github/oiltea/LMMain.java -d .
java -cp ".;lib/*" com.github.oiltea.LMMain
```

2. **Enter license details when prompted:**
```
License type: UE
Product ID: dbeaver-ue
Product version: 24.0
Owner ID: 12345
Owner company: MyCompany
Owner name: John Doe
Owner email: john@example.com
```

3. **Save the generated keys to `keys.txt`**

4. **Backup and update the plugin JAR:**
```bash
cd "C:\Users\User\AppData\Local\DBeaverUltimate\plugins"
copy com.dbeaver.app.ultimate_25.1.0.202506090822.jar com.dbeaver.app.ultimate_25.1.0.202506090822.jar.backup

cd C:\tmp
jar xf "C:\Users\User\AppData\Local\DBeaverUltimate\plugins\com.dbeaver.app.ultimate_25.1.0.202506090822.jar" keys/dbeaver-ue-public.key
# Replace key content in keys/dbeaver-ue-public.key with your PUBLIC KEY
jar uf "C:\Users\User\AppData\Local\DBeaverUltimate\plugins\com.dbeaver.app.ultimate_25.1.0.202506090822.jar" keys/dbeaver-ue-public.key
```

5. **Create license directory and files:**
```bash
mkdir "C:\Users\User\.jkiss-lm"
# Create public-key.txt and private-key.txt with the generated keys
```

6. **Block online validation:**
```bash
# Edit C:\Windows\System32\drivers\etc\hosts as Administrator
# Add: 127.0.0.1 dbeaver.com
```

7. **Launch DBeaver and import the LICENSE**

## Troubleshooting

- Ensure you're using the correct Java version (JDK 11+)
- Make sure the classpath includes the JAR files from the `lib/` directory
- Verify that the public key in the plugin JAR matches your generated PUBLIC KEY
- Check that `.jkiss-lm` directory exists in your user home with both key files
- Confirm that dbeaver.com is blocked in your hosts file