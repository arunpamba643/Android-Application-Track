# Static Analysis

Static Analysis involve with the apk file of the android application...here, we have to decompile the application to analys the application.

Tools involved in Static Analysis are\
MobSF : Open-source Auto-machine tool for static analysis\
JadX : Open-source application helps to decompile the application\
APKtool : Open source tool for decompiling the application

So, To perform the static analysis first we have to decompile the application. this process is also called as reverse engineering.

Use the JadX tool to decompile the application and look for the below files
1. Manifest file
2. signature file
3. Other files

### Manifest file
In mainifest file look for the below details of the application\
1. Minimum SDK Version : need above 29
2. ClearTextPassword : Needs to be in False
3. Deugging : Needs to be in False
4. Permissions : look for dangerous permissions like External read write, location, and memory related etc...
5. Export Activities : Look for the Export activities which are having value true

### Signature file
In Signature file look for the below details of the application\

1. Look for Algorithm and encryption Methods
2. Look for the Signature versions(mainy look for V2 it needs to be in true or else Jenus vulnerability may occure)

### Other Files

in other file look for Hard coded vulnerabilities like how the data being stored and what was the process etc...