# Trent's Apps

A small community app store for [umbrelOS](https://umbrel.com). It currently holds one app:

| App | What it does | Port |
| --- | --- | --- |
| **Bindery** | Ebook and audiobook manager, a modern Readarr replacement | 8787 |

These apps are unofficial and are not vetted by Umbrel. They are packaging only: the apps themselves belong to their original authors and keep their own licenses.

## Add this store to umbrelOS

1. Open the **App Store**.
2. Tap the three dots in the top-right and choose **Community App Stores**.
3. Paste this repository's URL and tap **Add**.
4. Find **Bindery** under "Trent's Apps" and install it.

## Bindery

[Bindery](https://github.com/vavallee/bindery) monitors authors, searches your indexers, sends downloads to your download client, then sorts and renames the finished files into your library.

### After installing

1. **Map your folders.** Go to **App settings → Bindery → Storage → Advanced → Add custom folder** and add one entry each:

   | Folder on your server | Path inside the app |
   | --- | --- |
   | Your downloads folder | `/downloads` |
   | Your ebooks folder | `/books` |
   | Your audiobooks folder (optional) | `/audiobooks` |

   Use the same `/downloads` path as your download client, so Bindery finds finished files without any path remapping.

2. **Create the admin account.** Open Bindery (port 8787). The first-run page sets up the login.
3. **Add a download client.** In **Settings**, add qBittorrent (or a Usenet client) using your server's LAN IP, not `127.0.0.1`, because inside a container that address points at Bindery itself. Set a category such as `books`.
4. **Add indexers.** Add Prowlarr or Jackett by its LAN address and API key.
5. **Test it.** Add one author and confirm a download is sent to your client and the file lands in `/books`.

### Troubleshooting

- **App won't start or can't write its settings:** the config folder may be owned by root. Fix it over SSH, then restart the app:

  ```
  sudo chown -R 1000:1000 /home/umbrel/umbrel/app-data/trent-bindery/data
  ```

- **Imports fail with permission errors:** make sure the user with ID 1000 can write to your books and downloads folders.
- **Prowlarr or qBittorrent can't be reached:** use the server's LAN IP and the app's port, not `localhost`.

### Notes

- The image uses the `latest` tag, so reinstalling can pull a newer version.
- Bindery has its own login, so Umbrel's extra login in front of it is switched off. This also lets other apps reach its API.

## Repository layout

```
umbrel-app-store.yml          store ID and name
trent-bindery/
  umbrel-app.yml              app listing
  docker-compose.yml          how umbrelOS runs the app
```

To add another app, create a new folder named `trent-<app>` with its own `umbrel-app.yml` and `docker-compose.yml`. The folder name must match the `id` inside `umbrel-app.yml`.

## Problems

If an install fails or the app won't start, open an issue and include the error message and your umbrelOS version.
