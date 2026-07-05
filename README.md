# AssetReuploader v6.0

## Latest release has mesh and animation support!

Do you: 

- Want to easily move all of your animations meshes or image assets from one group/profile to another?
- Have a lot of free marketplace image assets that you don't own and you want to use EditableImage?

## AssetReuploader is a simple Node.js tool for reuploading and replacing assets in your game through Roblox's Open Cloud. 

### This is GUI based with simple button clicks, meaning no fiddling around in command line interfaces

## What's new in v6.0

- **Fixed critical bugs** from v5.0
- **Improved error handling** throughout the codebase
- **Better timeout management** for long-running operations
- **CORS support** for cross-origin requests
- **Failures endpoint** added to easily retrieve failed assets
- **Removed Atomics.wait** blocking call (replaced with simple busy wait)
- **Enhanced logging** with better context
- **Added timeout protection** to all HTTP requests
- **Better file stream cleanup** to prevent memory leaks

## How to Use

When you hit search, it'll search for all assets under the selected instance or the datamodel (depending on your setting) that contains one of the selected content properties (only relevant for image uploads)

It'll only consider assets that the target creator (your profile or chosen group) doesn't already own

Then, when you hit Run, the node.js program should take care of the rest!

The node.js program works in 3 phases:

- Downloads the assets
- Reuploads the assets under the target creator
- Sends the results back to roblox

The roblox plugin then replaces all occurrences of those assetIDs in the game with the new ones! It also sets an "OldID" attribute on the instances

- ### For image uploads, if any image assets were moderated, it'll spit them out into a folder in the workspace for you so you can handle their reuploads manually if need be

## Setup

- Head over to releases and grab the latest release
  
- Drag AssetReuploader.rbxmx into a studio place, and right click -> Save as Local Plugin

## If you want to reupload image assets:

- Head to the Creator Dashboard https://create.roblox.com/dashboard/

- Navigate to Open Cloud for the creator you want to upload to (your profile or a group)

- Create a new API key

- Make sure your settings look correct, add your IP (or 0.0.0.0/0 if you don't care), and generate a new API Key

- Copy the full API key and then open up the plugin and paste it in the API Key field

- Make sure this is the correct API key for the group you have selected, or for your profile if Upload To Group is turned off

## Building from Source

- Install node.js
- Navigate to the project directory and open the terminal
- Enter these commands:
  ```
  npm install
  npm install -g pkg
  npm run build-exe
  ```

## API Endpoints

### POST /upload
Starts a reupload job
```json
{
  "assetIDs": ["rbxassetid://123"],
  "creatorID": "12345",
  "isGroup": false,
  "apiKey": "your-api-key",
  "uploadType": "Images",
  "cookie": "optional-cookie"
}
```

### GET /progress?jobId=<jobId>
Retrieve job progress

### GET /chunks?jobId=<jobId>&offset=0&count=50
Retrieve successful asset conversions in chunks

### GET /moderated?jobId=<jobId>&offset=0&count=50
Retrieve moderated assets in chunks

### GET /failures?jobId=<jobId>&offset=0&count=50
Retrieve failed assets in chunks

## Notes

- The conversion process can take several minutes for hundreds of assets. There are some long delays to avoid hitting OpenCloud rate limits. So relax and grab a coffee!

- This is a rewrite of the original AssetReuploader with bug fixes and improved stability.

## License

MIT
