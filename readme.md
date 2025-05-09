# 📚 Fetchly

<img src="static/media/logo.svg" alt="Fetchly" title="Fetchly" width="200" />

A clean, mobile-first responsive web interface for searching and requesting book downloads, forked from [Calibre-Web-Automated-Book-Downloader](https://github.com/crocodilestick/Calibre-Web-Automated). Fetchly streamlines the process of downloading books with a focus on modern design and mobile accessibility.

## ✨ Features

- 🌐 Clean, mobile-first responsive web interface for book search and download
- 🔄 Automated download to your specified ingest folder
- 🔌 Seamless integration with Calibre-Web-Automated
- 📖 Support for multiple book formats (epub, mobi, azw3, fb2, djvu, cbz, cbr)
- 🛡️ Cloudflare bypass capability for reliable downloads
- 🐳 Docker-based deployment for quick setup

## 🖼️ Screenshots

### Mobile Views
<img src="README_images/search-mobile.webp" alt="Mobile search interface" title="Mobile search interface" width="300" />
<img src="README_images/details-mobile.webp" alt="Mobile details modal" title="Mobile details modal" width="300" />
<img src="README_images/downloading-mobile.webp" alt="Mobile download queue" title="Mobile download queue" width="300" />

### Desktop Views
<img src="README_images/search-desktop.webp" alt="Desktop search interface" title="Desktop search interface" width="600" />
<img src="README_images/details-desktop.webp" alt="Desktop details modal" title="Desktop details modal" width="600" />

## 🚀 Quick Start

### Prerequisites

- Docker
- Docker Compose
- A running instance of [Calibre-Web-Automated](https://github.com/crocodilestick/Calibre-Web-Automated) (recommended)

### Installation Steps

1. Get the docker-compose.yml:

   ```bash
   curl -O https://raw.githubusercontent.com/lklynet/fetchly/refs/heads/main/docker-compose.yml
   ```

2. Start the service:

   ```bash
   docker compose up -d
   ```

3. Access the web interface at `http://localhost:8084`

## ⚙️ Configuration

### Environment Variables

#### Application Settings

| Variable          | Description             | Default Value      |
| ----------------- | ----------------------- | ------------------ |
| `FLASK_PORT`      | Web interface port      | `8084`             |
| `FLASK_DEBUG`     | Debug mode toggle       | `false`            |
| `FLASK_HOST`      | Web interface binding   | `0.0.0.0`          |
| `INGEST_DIR`      | Book download directory | `/cwa-book-ingest` |
| `UID`             | Runtime user ID         | `1000`             |
| `GID`             | Runtime group ID        | `100`              |
| `ENABLE_LOGGING`  | Enable log file         | `true`             |

If logging is enabld, log folder default location is `var/log/cwa-book-downloader`

#### Download Settings

| Variable               | Description                                               | Default Value                     |
| ---------------------- | --------------------------------------------------------- | --------------------------------- |
| `MAX_RETRY`            | Maximum retry attempts                                    | `3`                               |
| `DEFAULT_SLEEP`        | Retry delay (seconds)                                     | `5`                               |
| `MAIN_LOOP_SLEEP_TIME` | Processing loop delay (seconds)                           | `5`                               |
| `SUPPORTED_FORMATS`    | Supported book formats                                    | `epub,mobi,azw3,fb2,djvu,cbz,cbr` |
| `BOOK_LANGUAGE`        | Preferred language for books                              | `en`                              |
| `AA_DONATOR_KEY`       | Optional Donator key for Anna's Archive fast download API | ``                                |

Note that PDF are NOT supported at the moment (they do not get ingested by CWA, but if you want to just download them locally, you can add `pdf` to the `SUPPORTED_FORMATS` env)  
If you change `BOOK_LANGUAGE`, you can add multiple comma separated languages, such as `en,fr,ru` etc.  

#### AA 

| Variable               | Description                                               | Default Value                     |
| ---------------------- | --------------------------------------------------------- | --------------------------------- |
| `AA_BASE_URL`          | Base URL of Annas-Archive (could be changed for a proxy)  | `https://annas-archive.org`       |
| `USE_CF_BYPASS`        | Disable CF bypass and use alternative links instead       | `true`                           |

If you are a donator on AA, you can use your Key in `AA_DONATOR_KEY` to speed up downloads and bypass the wait times.
If disabling the cloudflare bypass, you will be using alternative download hosts, such as libgen or z-lib, but they usually have a delay before getting the more recent books and their collection is not as big as aa's. But this setting should work for the majority of books.

#### Network Settings

| Variable               | Description                   | Default Value           |
| ---------------------- | ----------------------------- | ----------------------- |
| `CLOUDFLARE_PROXY_URL` | Cloudflare bypass service URL | `http://localhost:8000` |
| `PORT`                 | Container external port       | `8084`                  |

`CLOUDFLARE_PROXY_URL` is ignored if `USE_CF_BYPASS` is set to `false`

### Volume Configuration

```yaml
volumes:
  - /your/local/path:/cwa-book-ingest
```

Mount should align with your Calibre-Web-Automated ingest folder.

## 🏗️ Architecture

The application consists of two key services:

1. **Fetchly**: Main application providing web interface and download functionality
2. **cloudflarebypassforscraping**: Support service for handling Cloudflare-protected websites

## 🏥 Health Monitoring

Built-in health checks monitor:

- Web interface availability
- Download service status
- Cloudflare bypass service connection

Checks run every 30 seconds with a 30-second timeout and 3 retries.

## 📝 Logging

Logs are available in:

- Container: `/var/logs/calibre-web-automated-bookdownloader.log`
- Docker logs: Access via `docker logs`

## 🤝 Contributing

Contributions are welcome! Feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚠️ Important Disclaimers

### Copyright Notice

While this tool can access various sources including those that might contain copyrighted material (e.g., Anna's Archive), it is designed for legitimate use only. Users are responsible for:

- Ensuring they have the right to download requested materials
- Respecting copyright laws and intellectual property rights
- Using the tool in compliance with their local regulations

### Duplicate Downloads Warning

Please note that the current version:

- Does not check for existing files in the download directory
- Does not verify if books already exist in your Calibre database
- Exercise caution when requesting multiple books to avoid duplicates

## 💬 Support

For issues or questions, please file an issue on the GitHub repository.
