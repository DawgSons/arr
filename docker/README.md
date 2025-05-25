# Docker Deployment

This directory contains the Docker Compose configuration for deploying the arr-stack media server.

## Components

- **Radarr**: Movie management
- **Sonarr**: TV show management
- **Bazarr**: Subtitle management
- **SABnzbd**: Usenet downloader
- **Prowlarr**: Indexer manager
- **Jellyfin**: Media server
- **Heimdall**: Application dashboard
- **YTPlaylistarr**: Custom YouTube playlist downloader

## Deployment Instructions

To deploy the entire stack:

```bash
# Start all services
docker-compose up -d

# Check service status
docker-compose ps

# View logs for a specific service
docker-compose logs -f radarr
```

To stop the stack:

```bash
# Stop all services
docker-compose down
```

## Configuration

### Volumes

The Docker Compose setup uses the following volume mappings:

- `/docker/appdata/{service}:/config` - Configuration data
- `/data/usenet/downloads:/data/usenet/downloads` - Download directory
- `/media/nucy/media:/data/usenet/media` - Media library
- `/etc/localtime:/etc/localtime:ro` - Host system time

### Ports

Each service exposes its web interface on a specific port:

- Radarr: 7878
- Sonarr: 8989
- Bazarr: 6767
- SABnzbd: 8080, 9090
- Prowlarr: 9696
- Jellyfin: 8097
- Heimdall: 80, 443

### Environment Variables

Common environment variables:

- `PUID=1000` - User ID
- `PGID=1000` - Group ID
- `UMASK=002` - File permission mask
- `TZ=Europe/Amsterdam` - Timezone

## Custom Components

### YTPlaylistarr

A custom component that downloads YouTube playlists:

1. Configuration:
   - Edit `ytplaylistarr/playlists.txt` to add YouTube playlist URLs
   - Adjust check frequency with the `SLEEP_AMOUNT` environment variable

2. Build and start:
   ```bash
   # Build the image
   docker-compose build ytplaylistarr
   
   # Start the service
   docker-compose up -d ytplaylistarr
   ```

## Troubleshooting

If you encounter issues:

1. Check container logs:
   ```bash
   docker-compose logs -f [service_name]
   ```

2. Verify volume permissions:
   ```bash
   ls -la /docker/appdata
   ls -la /data/usenet/downloads
   ls -la /media/nucy/media
   ```

3. Restart a specific service:
   ```bash
   docker-compose restart [service_name]
   ```
