Even though the microservices are rather light, the application consists of many services, each running in a separate container. It also includes SQL Server, Redis, MongoDb, RabbitMQ and Seq as separate containers. The SQL Server container has four databases (for different microservices) and takes a significant amount of memory. It all adds up.

This is why we recommend:
- Running eShopOnContainers on a machine **with at least 16 GB of RAM**. 
- Docker Desktop [**should be running in WSL 2 mode**](https://docs.docker.com/desktop/windows/wsl/). It is the default mode in newer Docker Desktop versions, and it provides significantly better performance than the old, Hyper-V based mode.

## Low memory configuration

If your computer only has 8 GB RAM, you **might** still get eShopOnContainers up and running, but it's not sure and you will not be able to run Visual Studio. You might be able to run VS Code and you'll be limited to the CLI. You might even need to run Chromium or any other bare browser, Chrome will most probably not do. You'll also need to close any other program running in your machine.

The easiest way to get Chromium binaries directly from Google is to install the [node-chromium package](https://www.npmjs.com/package/chromium) in a folder and then look for the `chrome.exe` program, as follows:

1. Install node.

2. Create a folder wherever best suits you.

3. Run `npm install --save chromium`

4. After installation finishes go to folder `node_modules\chromium\lib\chromium\chrome-win` (in Windows) to find `chrome.exe`


## Additional resources

- **Changing WSL 2 VM parameters via wslconfig (including Docker VM)** \
  <https://learn.microsoft.com/en-us/windows/wsl/wsl-config#configuration-setting-for-wslconfig>

- **Troubleshoot Visual Studio development with Docker (Networking)** \
  <https://docs.microsoft.com/en-us/visualstudio/containers/troubleshooting-docker-errors#errors-specific-to-networking-when-debugging-your-application>
