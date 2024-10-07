#### Process of getting SpotDL configured and running on Unraid.

##### Fork, customise, build SpotDL
- clone https://github.com/spotDL/spotify-downloader
- customise Dockerfile and docker compose (docker compose is for testing)
- Build and upload to Github Registry
`docker build -t spotdl .`

- setup a personal access token before logging into ghcr.io
`docker login -u tinmice --password YOUR_PERSONAL_ACCESS_TOKEN ghcr.io`

- tag docker image and push
```
docker tag spotdl ghcr.io/tinmice/spotify-downloader/spotdl:latest
docker push ghcr.io/tinmice/spotify-downloader/spotdl:latest
```

##### Setting up on Unraid
- login to git registry through docker to access registry
`docker login -u tinmice --password YOUR_PERSONAL_ACCESS_TOKEN ghcr.io`
`docker pull ghcr.io/tinmcie/spotify-downloader/spotdl:latest`

- Create repository using add container and manually add the fields
- Add repository and initiate

#### Setup SpotDL
- cookies.txt from music.youtube.com (in SpotDL instructions)
- setup Sptify app on developer site to obtain client id and secret
- update config.json and copy along with cookies to appdata folder
- initiate and run through authentication process
 - currently unclear, seemed to work even though the processes didn't complete, auth-user=no, headless=yes


##### Automate
- automate sync command to trigger sync
- automate RClone update to sync to OneDrive - authenticate on install9

``` Unraid script
#!/bin/bash

docker run --rm --name spotdl-ben \
    -v /mnt/user/ardanan/Music/Ripped/Ben:/music \
    -v /mnt/user/appdata/spotify-downloader/ben:/sync \
    -v /mnt/user/appdata/spotify-downloader/ben/config.json:/root/.spotdl/config.json \
    -v /mnt/user/appdata/spotify-downloader/ben/cookies.txt:/cookies.txt \
    ghcr.io/tinmice/spotify-downloader/spotdl \
    sync 'sync/ripped.spotdl' > 'music/0 errors.txt' # switch to this to point to established sync file
    # sync https://open.spotify.com/playlist/4c44FeMLFq0x2YV557HmKs --save-file 'sync/ripped.spotdl' > 'music/0 errors.txt' # run first time to sync playlist and create sync file
    
sleep 5

rclone sync mnt/user/ardanan/Music/Ripped/Ben BenDrive:/Music/Ripped/Ben
```

#### Onboard new people:
- spotify playlist
- spotify app in developer portal
- cookies.txt from YT, config.json from repo
create folder structure, copy script
- share onedrive folder