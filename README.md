# Jellyfin Symlink Helper Lite

Note : this is a lite version (no docker) of the original Vijai Djearam's repository [jellyfin-symlink-helper](https://github.com/vijaidjearam/jellyfin-symlink-helper)

## 📦 Overview

This script :
- Rename media files using [guessit](https://github.com/guessit-io/guessit)
- Create symbolic links in a structure compatible with [Jellyfin](https://jellyfin.org/)
- Clean up broken symlinks automatically
- Only process newly added or recently modified files

So this helper tool is useful when downloaded or unmanaged media needs to be cleaned up, renamed, and symlinked into a structured format for Jellyfin to consume easily.

---

### 1. Install the dependencies

```bash
sudo apt install python3-guessit python3-dotenv
```

### 2. Clone the repository

```bash
git clone https://github.com/brahimmachkouri/jellyfin-symlink-helper.git
cd jellyfin-symlink-helper
```

### 3. Set up the environment variables in the .env file

Create a `.env` file in the same directory as the python file and define the following variables:

| Variable    | Description                             | Example              |
|-------------|-----------------------------------------|----------------------|
| `SOURCE`    | Path to the source media directory      | `/mnt/media`     |
| `DEST_BASE` | Path to the destination directory       | `/srv/jellyfin`      |

### Example `.env` file

```env
SOURCE=/mnt/media
DEST_BASE=/srv/jellyfin
```

In jellyfin, define the DEST_BASE as your media library path.

> 🔒 **Note**: Ensure appropriate permissions are set so the script can read from `SOURCE` and write to `DEST_BASE`.

## 🔁 Automate with cron
To run the script periodically:

```bash
crontab -e
```

Then add this line :
```
0 * * * * /usr/bin/python3  /path/to/jellyfin-symlink-helper/rename_and_symlink.py >> /path/to/jellyfin-symlink-helper/symlink_processor.log 2>&1
```


## 🛠 How It Works
- Files in **$SOURCE** modified within the last **$MODIFIED_WITHIN_HOURS** hours are scanned.

- Their names are parsed with **guessit**.

- Symlinks are created in **$DEST_BASE** following Jellyfin-compatible structure:

    - Movies: **/Movies/Title (Year)/Title (Year).ext**

    - Tv Shows: **/TV Shows/Show/Season 01/Show - S01E01.ext**

- Broken symlinks in **$DEST_BASE** are automatically deleted.

## 🛠️ Troubleshooting

- **No symlinks created?**
  - Ensure your source folder is not empty.
  - Validate file naming for compatibility.
