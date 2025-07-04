🚀 The "One-Touch" Installation Workflow
This guide walks you through the entire process, from blank servers to a running bot.


Step 1: Prepare Your Servers
On both of your fresh Ubuntu 22.04 servers, perform the following initial setup:	

1.	Update the system:

sudo apt update && sudo apt upgrade -y
 

2.	Install Python, Pip, and Git:

sudo apt install python3 python3-pip git -y
 

3.	Download Project-Aegis.py and copy over to your previously setup machines. Make sure it goes in a folder as there are log files generated.

4.	Install Python Dependencies:

pip3 install -r requirements.txt

Step 2: Install Pterodactyl Panel
On the machine designated to be your Panel server, run the script with the --install-panel flag. This will begin an automated installation of the Pterodactyl Panel and all its dependencies (Nginx, MariaDB, Redis, etc.).

python3 Project-Aegis.py --install-panel
 

The script will prompt you for necessary information, such as your domain name, database credentials, and an admin user account for the panel. Follow the on-screen instructions carefully.

Step 3: Install Pterodactyl Wings
On the machine designated to be your Wings server, run the script with the --install-wings flag. This will install the Wings daemon, which is responsible for running your game servers.

python3 Project-Aegis.py --install-wings
 
The script will ask for the URL of your new Pterodactyl Panel and an auto-generated token to link the two systems. It will guide you through creating a new "Node" in your panel and pasting the configuration.

Step 4: Verify Connection and Configure the Bot
Once both installations are complete, log into your Pterodactyl Panel at the domain you configured.

  1.	Import the Egg (Future: Automatic):
o	For now, this is the only manual step. Follow the Egg Import Guide below to install the Universal Installer Egg. In future versions, the bot will handle this automatically upon first run.

  2.	Generate API Keys:
o	Application API Key: Go to Admin -> Application API, create a key with Read & Create permissions for Servers, and Read permissions for all other categories.
o	Client API Key: Go to your user icon -> Account -> API Credentials, and create a new key.

  3.	Configure the Bot:
o	On your Panel server (or a third machine), copy config.json.example to config.json.
o	Fill in all the required values:
Generated json
{
  "DISCORD_BOT_TOKEN": "YOUR_DISCORD_BOT_TOKEN_HERE",
  "PTERODACTYL_PANEL_URL": "https://panel.yourdomain.com",
  "PTERO_APP_API_KEY": "YOUR_APPLICATION_API_KEY_HERE",
  "PTERO_CLIENT_API_KEY": "YOUR_CLIENT_API_KEY_HERE",
  "CURSEFORGE_API_KEY": "YOUR_CURSEFORGE_API_KEY_HERE",
  "server_creation_defaults": {
    "nest_id_minecraft": "1", // Default Nest ID for Minecraft
    "universal_installer_egg_id": "ID_OF_THE_IMPORTED_EGG"
  }
}
 
Use code with caution.Json
Step 5: Run the Bot
Now, you can run the bot in its primary monitoring mode. It's highly recommended to use a process manager like screen or systemd to ensure the bot runs 24/7.
Using screen (simple method):

# Start a new screen session named "discordbot"
screen -S discordbot

# Run the bot inside the session
python3 Project-Aegis.py

# Detach from the session by pressing Ctrl+A, then D.
# The bot will keep running. To re-attach later, use: screen -r Project-Aegis
 

Your system is now fully operational!

___________________________________________________________________________________________________________________________________________________________________
🎮 Using the Bot to Install a Modpack
With the bot running, you can deploy servers from any Discord channel it has access to.
Command: /install_server

Parameters:
•	server_key: A short, unique key for the server (e.g., atm9).
•	server_name: The display name for the server (e.g., All The Mods 9).
•	project_id: The CurseForge Project ID of the modpack.
•	file_id (Optional): A specific CurseForge File ID to install a specific version.
Example 1: Auto-installing the latest server pack
Leave the file_id option blank. The script will automatically find the best server pack available.
/install_server server_key:atm9 server_name:All The Mods 9 project_id:715572
Example 2: Installing a specific version
Find the numerical File ID from the CurseForge "Files" tab and provide it.
/install_server server_key:tek3 server_name:Techopolis 3 project_id:1193409 file_id:6686603
The bot will handle the rest. It will create the server in Pterodactyl, run the installer, and notify you when it's complete and starting up.

❓ Troubleshooting

•	API Request resulted in errors... must be a string: This is a legacy error. If you encounter it, ensure your Egg's variable validation for MODPACK_FILE_ID is set to nullable|string in the Pterodactyl Panel.
•	Server crashes on startup: The installation was successful, but the modpack itself has issues (e.g., mod conflicts). This is not an installer error. Check the server console and the modpack's official issue page for solutions.
•	Installation fails with No such file or directory: This can happen if the pack author has made a critical error in their server pack zip. Report the issue on the project's GitHub.

❤️ Contributing & Support

This is a community-driven project, and we welcome contributions. Found a bug or have a feature request that would make this suite even better?
•	Report Bugs: Please open an issue on our GitHub Issues page. Include as much detail as possible, including logs from both the bot and the Pterodactyl server installation console.

•	Feature Requests: We'd love to hear your ideas! Open an issue and describe the new functionality you'd like to see.
•	Pull Requests: If you'd like to contribute code, please fork the repository and submit a pull request.

For direct support, please join our Official Discord Server at https://discord.gg/GF3YSbrR3r.

