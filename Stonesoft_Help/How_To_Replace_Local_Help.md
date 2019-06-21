# How to use the online Help from a local server or computer

  - You can configure the Management Client to use a copy of the online
   Help from your own computer or from a server in the local network.

  - By default, the Management Client’s online Help is accessed through
   the Internet. When you use the online Help locally, the online Help is available even when there is no Internet connectivity.

        Note: When you use a local copy of the online Help, you must manually update the online Help when a new version is available.


   1. Download the online Help .zip file for your release from
   [stonsoft.com/onlinehelp](http://⁠help.stonesoft.com/onlinehelp/StoneGate/SMC/)

   2. Extract the .zip file to a suitable location in your local network.
   You can also extract the Help file to a share or your local intranet server.

   3. On the computer where you use the Management Client, browse to the <user home>/.stonegate/data folder.

      Example: In Windows, browse to C:\Users\<user_name>\.stonegate.


      Note: If the Management Client is open, close it before editing the SGClientConfiguration.txt file.

  4. Edit the SGClientConfiguration.txt file.

  5. Add a parameter "HELP_SERVER_URL=" and enter the path to the main folder under which the online Help files are stored as the value for the parameter.
    - If you extracted the online Help on the same computer where you use the Management Client, enter "file:///" and the path.

      Example: If you extracted the online Help on your own computer to C:\help\ngfw_620_help_en-us, enter

      HELP_SERVER_URL=file:///C:/help/ngfw_620_help_en-us


      Note: Use only forward slashes (/) in the URL even if the operating system uses backslashes (\) in file paths.
      If the path contains spaces, replace them with %20 in the URL.


  - If you extracted the online Help on a server in the local network, enter http:// and the path.

      Example: If you extracted the online Help to a folder called ngfw_620_help_en-us on an intranet server, enter

      HELP_SERVER_URL=http://<intranet.server>/ngfw_620_help_en-us.

6. Save the changes to the SGClientConfiguration.txt file.

7. Start the Management Client
