# docker ionic sample code

```bash
$PROJECT = [path/to/project]
$PROXY = [Proxy]
$GRADLE_PROPERTIES = [path/to/gradle.properties]

if(test-path platforms){ # Whether platforms file exists in project directory.
  write-host "platfrom has been already existed."
}else{
  write-host "Creating platforms"
  docker run -ti --rm -p 8100:8100 -p 35729:35729 `
      -v ${PROJECT}:/Sources:rw `
      -v ${GRADLE_PROPERTIES}:/root/.gradle/gradle.properties `
      --env http_proxy=${PROXY} `
      --env https_proxy=${PROXY} `
      marcoturi/ionic cordova platform add android
}

write-host "Creating apk."
docker run -ti --rm -p 8100:8100 -p 35729:35729 `
    -v ${PROJECT}:/Sources:rw `
    -v ${GRADLE_PROPERTIES}:/root/.gradle/gradle.properties `
    --env http_proxy=${PROXY} `
    --env https_proxy=${PROXY} `
    marcoturi/ionic cordova build

write-host "Installing app."
adb install -r .\platforms\android\app\build\outputs\apk\debug\app-debug.apk

```