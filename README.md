# Password Manager

A single-file, offline password manager with an Authenticator-style interface. It is built with HTML, CSS, JavaScript, AJAX (`XMLHttpRequest`) and JSON. Everything runs in your browser, and nothing is sent to a server.

## Features

- **Master password lock:** the vault is encrypted with AES-256-GCM, using a key derived from your master password with PBKDF2 (SHA-256, 150,000 rounds). It auto-locks after inactivity.
- **Passwords tab:** search, category and favorite filters, a "Weak" filter, and one-tap copy for username and password. The clipboard is cleared after 30 seconds.
- **Add and edit accounts:** name, username, password, website, category, notes and an optional 2FA secret. It includes a strength meter, show/hide toggle and a generate button.
- **Authenticator codes:** live 6-digit TOTP codes (RFC 6238) with a 30-second countdown ring.
- **Password generator:** adjustable length (6-48), character sets, strength bar, copy, and "save as new account".
- **Security audit:** counts weak and reused passwords.
- **Import and export:** copy-and-paste JSON.
- **Themes:** light, dark, or follow the system.
- **Responsive layout:** works on phones and desktops.

## Getting started

1. Download `password-manager.html`.
2. Open it in a modern browser. No build step, server or internet connection is needed.
3. On first launch, create a master password (at least 8 characters). A sample account is added so you can see how it looks. Edit or delete it.

## Project structure

```
password-manager.html   HTML, CSS and JavaScript in one file
LICENSE                 MIT license
README.md               This file
```

## How it works

| Part | Technology |
| --- | --- |
| Interface | HTML and CSS (custom properties for theming) |
| Icons | Inline SVG in the glyphicon style, using the `glyphicon` class |
| Logic | Vanilla JavaScript |
| AJAX | `XMLHttpRequest` loads a JSON service catalog (brand names and colors), with an inline fallback if the request is blocked |
| Storage | Browser `localStorage`, holding only the encrypted vault |
| Cryptography | Web Crypto API (AES-GCM, PBKDF2, HMAC-SHA1 for TOTP) |

### Data format

Exports use this JSON structure:

```json
{
  "app": "password-manager",
  "version": 1,
  "entries": [
    {
      "id": 1700000000000,
      "name": "GitHub",
      "user": "you@example.com",
      "pass": "••••••••",
      "url": "https://github.com",
      "notes": "",
      "fav": false,
      "secret": "JBSWY3DPEHPK3PXP",
      "cat": "Work"
    }
  ]
}
```

Import also accepts a plain array of entries. Only `name` is required.

## Changing the master password

There is no built-in option yet. To change it:

1. Unlock the vault and open **Settings**.
2. Tap **Export JSON** and copy the text somewhere safe.
3. Tap **Erase vault** and confirm.
4. Create a new vault with the new master password.
5. Open **Settings**, tap **Import JSON**, and paste the copied text.

Delete the copied text afterwards, because exports are plain text.

## Limits and requirements

- **Capacity:** the 5 MB `localStorage` limit allows roughly 7,000-10,000 records. About 1,000-2,000 records is comfortable, because the list has no virtual scrolling. These are estimates, not tested numbers.
- **Browsers:** any modern browser with Web Crypto (roughly Chrome or Edge 80+, Firefox 74+, Safari 13.1+).
- **Context:** HTTPS, `localhost`, or a local file. Encryption does not work over plain HTTP.
- **Storage must be enabled:** private windows usually erase the vault on close, and clearing site data deletes it.

## Security notes

- A forgotten master password cannot be recovered. The only option is to erase the vault.
- Exported JSON is **not encrypted**. Store it safely and delete it when you no longer need it.
- Data lives only in the browser on the device where you created it. Export a backup regularly.
- This is a personal project and has not had an independent security audit. For critical accounts, consider an established password manager.

## Roadmap ideas

- A "Change master password" button that re-encrypts the vault in place
- Paged lists and IndexedDB storage for very large vaults
- Encrypted export and import
- Password history per account

## License

Released under the [MIT License](LICENSE).
