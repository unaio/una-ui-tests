# UNA UI Test Automation

## Running/Debugging tests

- Install .Net Core & Mono
- VSCode with extensions: C#, Ionide-fsharp, Neptune

To run tests you need to set environment variable `adminexamplepassword` which is used for the 16 testing accounts. You may add it to your `.bashrc` file.

    export adminexamplepassword=somepassword

To run tests in terminal run `dotnet test`.
On a Docker host machine you may run `./runTests.sh` to run tests in Docker angainst Selenium Grid.

Hint:
This command can be used to change owner of the files in `TestResuls` folder,
to be able to see images of the failed tests to get an idea why they are failed.
In the 2nd step file extensions are changed as for some reason canopy saves them as jpg,
while in fact the images are PNG.

```bash
sudo chown -R $USER TestResults && find ./TestResults -depth -name "*.jpg" -exec sh -c 'mv "$1" "${1%.abc}.png"' _ {} \;
```

### Debugging tests with VCanopy source

- Get source of VCanopy 
- Open `una-ui-tests.fsproj` and in Visual Studio menu in the `Solution Configurations` dropdown select configuration `DebugVCanopySourceYes`.
- Build solution

### Running in Zalenium

#### Run on a docker host

```
    docker pull elgalu/selenium
    
    # Pull Zalenium
    docker pull dosel/zalenium
    
    # Run it!
    docker run --rm -ti --name zalenium -p 4444:4444 \
      -v /var/run/docker.sock:/var/run/docker.sock \
      -v /tmp/videos:/home/seluser/videos \
      --privileged dosel/zalenium start
```

#### Change `una-ui-tests\Tests\TestsSetup.fs`

replace this `setDriverFactory createChromeDriver` with `setDriverFactory createRemoteDriver`
update createRemoteDriver to point to proper Zalenium url. Which can be found in the output line like the one below

```
03:16:18.686 [main] INFO  org.openqa.grid.web.Hub - Clients should connect to http://172.17.0.3:4445/wd/hub
```



### powershell chrome stop process

get-process -name ChromeDriver | stop-process

### Run grid only

docker-compose up --scale test=0
