
<div align="center">

# @vyzensockets/baileys

[![npm version](https://img.shields.io/npm/v/@vyzensockets/baileys.svg?style=flat-square)](https://www.npmjs.com/package/@vyzensockets/baileys)
[![npm downloads](https://img.shields.io/npm/dt/@vyzensockets/baileys.svg?color=blueviolet&label=Downloads&logo=npm&style=flat-square)](https://www.npmjs.com/package/@vyzensockets/baileys)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![WhatsApp](https://img.shields.io/badge/WhatsApp-API-25D366?style=flat-square&logo=whatsapp)](https://whatsapp.com)

<img src="https://files.catbox.moe/7m4cuw.jpg" width="100%" style="border-radius: 10px; margin: 20px 0;" alt="Cover Banner" />

<p align="center">
  <strong>Lightweight, Amazing, and Powerful WhatsApp Web API for Node.js</strong><br>
  Built for stability and ease of use.
</p>

[Bug Report](https://github.com/vyzensockets/baileys/issues) · [Feature Request](https://github.com/vyzensockets/baileys/issues) · [Join Channel](https://whatsapp.com/channel/0029Vb69z8n1dAvztHQTDu3r)

</div>

---

## ⚡ Installation

Install the package using npm or yarn:

```bash
npm install @vyzensockets/baileys
# or
yarn add @vyzensockets/baileys

package.json Setup
"dependencies": {
  "baileys": "npm:@vyzensockets/baileys"
}

🚀 Quick Start
<details>
<summary><b>🔌 Basic Connection (QR Code)</b></summary>


import makeWASocket, { useMultiFileAuthState } from '@vyzensockets/baileys'

async function connectToWhatsApp () {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys')
    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true
    })

    sock.ev.on('creds.update', saveCreds)
    
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update
        if(connection === 'close') {
            const shouldReconnect = (lastDisconnect.error)?.output?.statusCode !== DisconnectReason.loggedOut
            console.log('connection closed due to ', lastDisconnect.error, ', reconnecting ', shouldReconnect)
            if(shouldReconnect) {
                connectToWhatsApp()
            }
        } else if(connection === 'open') {
            console.log('opened connection')
        }
    })
}

connectToWhatsApp()

</details>
<details>
<summary><b>📱 Connection with Pairing Code (No QR)</b></summary>


> Note: Pairing Code requires printQRInTerminal: false
> 
import makeWASocket, { useMultiFileAuthState } from '@vyzensockets/baileys'

const { state, saveCreds } = await useMultiFileAuthState('auth_info_baileys')
const sock = makeWASocket({
    auth: state,
    printQRInTerminal: false, // MUST BE FALSE
    browser: ["Ubuntu", "Chrome", "20.0.04"]
})

if (!sock.authState.creds.registered) {
    const phoneNumber = '6289526377530' // Input number here
    const code = await sock.requestPairingCode(phoneNumber)
    console.log(`Your Pairing Code: ${code}`)
}

sock.ev.on('creds.update', saveCreds)

</details>
📨 Sending Messages
Click on the buttons below to view the code examples.
Basic Messages
<details>
<summary><b>💬 Text Message</b></summary>


await sock.sendMessage(jid, { text: 'Hello World! 🌍' })

</details>
<details>
<summary><b>↩️ Quote (Reply) Message</b></summary>


await sock.sendMessage(jid, { text: 'This is a reply!' }, { quoted: m })

</details>
<details>
<summary><b>🏷️ Mention User</b></summary>


await sock.sendMessage(jid, { 
    text: 'Hello @6289526377530', 
    mentions: ['6289526377530@s.whatsapp.net'] 
})

</details>
<details>
<summary><b>📍 Location & Live Location</b></summary>


Static Location:
await sock.sendMessage(jid, { 
    location: { degreesLatitude: 24.121231, degreesLongitude: 55.1121221 } 
})

Live Location:
await sock.sendMessage(jid, { 
    location: { degreesLatitude: 24.121231, degreesLongitude: 55.1121221 },
    live: true 
})

</details>
<details>
<summary><b>👤 Contact Card (VCard)</b></summary>


const vcard = 'BEGIN:VCARD\n' 
            + 'VERSION:3.0\n' 
            + 'FN:Vyzen User\n' 
            + 'ORG:Vyzen Sockets\n' 
            + 'TEL;type=CELL;type=VOICE;waid=6289526377530:+62 895 2637 7530\n' 
            + 'END:VCARD'

await sock.sendMessage(jid, { 
    contacts: { 
        displayName: 'Vyzen', 
        contacts: [{ vcard }] 
    }
})

</details>
<details>
<summary><b>❤️ Reaction Message</b></summary>


await sock.sendMessage(jid, {
    react: {
        text: '💖', 
        key: message.key
    }
})

</details>
Media Messages
<details>
<summary><b>🖼️ Image Message</b></summary>


await sock.sendMessage(jid, { 
    image: { url: '[https://example.com/image.png](https://example.com/image.png)' }, 
    caption: 'Look at this picture!' 
})

</details>
<details>
<summary><b>🎥 Video Message</b></summary>


await sock.sendMessage(jid, { 
    video: { url: '[https://example.com/video.mp4](https://example.com/video.mp4)' }, 
    caption: 'Watch this!',
    gifPlayback: false // Set true for GIF
})

</details>
<details>
<summary><b>🎵 Audio / Voice Note</b></summary>


await sock.sendMessage(jid, { 
    audio: { url: './audio.mp3' }, 
    mimetype: 'audio/mp4',
    ptt: true // Set true for Voice Note (waveform)
})

</details>
<details>
<summary><b>📄 Document/File Message</b></summary>


await sock.sendMessage(jid, { 
    document: { url: './file.pdf' }, 
    mimetype: 'application/pdf', 
    fileName: 'invoice.pdf'
})

</details>
<details>
<summary><b>📦 Sticker Message</b></summary>


await sock.sendMessage(jid, { 
    sticker: { url: './sticker.webp' } 
})

</details>
Interactive & Special Messages
<details>
<summary><b>📊 Poll Message</b></summary>


await sock.sendMessage(jid, {
    poll: {
        name: 'Which do you prefer?',
        values: ['Coffee', 'Tea', 'Water'],
        selectableCount: 1
    }
})

</details>
<details>
<summary><b>📌 Pin Message</b></summary>


await sock.sendMessage(jid, {
    pin: {
        type: 1, // 1 to pin, 2 to unpin
        time: 86400, // 24 hours
        key: targetMessageKey
    }
})

</details>
<details>
<summary><b>🔘 Button Message (Interactive)</b></summary>


await sock.sendMessage(jid, {
    text: 'Please select an option:',
    footer: 'Vyzen Bot',
    buttons: [
        { buttonId: 'id1', buttonText: { displayText: 'Option A' }, type: 1 },
        { buttonId: 'id2', buttonText: { displayText: 'Option B' }, type: 1 }
    ],
    headerType: 1
})

</details>
<details>
<summary><b>📑 List Message (Interactive)</b></summary>


const sections = [
    {
        title: 'Section 1',
        rows: [
            { title: 'Option 1', rowId: 'opt1', description: 'Description here' },
            { title: 'Option 2', rowId: 'opt2', description: 'Description here' }
        ]
    }
]

await sock.sendMessage(jid, {
    text: 'Select from list',
    buttonText: 'Click Me',
    sections
})

</details>
🛠️ Group Management
<details>
<summary><b>🆕 Create Group</b></summary>


const group = await sock.groupCreate('My Cool Group', ['628xxx@s.whatsapp.net', '628xxx@s.whatsapp.net'])
console.log('Created Group ID:', group.gid)

</details>
<details>
<summary><b>➕ Add/Remove Participants</b></summary>


// Add
await sock.groupParticipantsUpdate(jid, ['user@s.whatsapp.net'], 'add')
// Remove
await sock.groupParticipantsUpdate(jid, ['user@s.whatsapp.net'], 'remove')

</details>
<details>
<summary><b>👮 Promote/Demote Admins</b></summary>


// Promote
await sock.groupParticipantsUpdate(jid, ['user@s.whatsapp.net'], 'promote')
// Demote
await sock.groupParticipantsUpdate(jid, ['user@s.whatsapp.net'], 'demote')

</details>
<details>
<summary><b>✏️ Update Group Name & Description</b></summary>


await sock.groupUpdateSubject(jid, 'New Group Name')
await sock.groupUpdateDescription(jid, 'New Description Here')

</details>
<details>
<summary><b>⚙️ Group Settings</b></summary>


// Close Group (Admins only)
await sock.groupSettingUpdate(jid, 'announcement')
// Open Group (Everyone)
await sock.groupSettingUpdate(jid, 'not_announcement')

</details>
🔒 Privacy & Blocking
<details>
<summary><b>🚫 Block/Unblock User</b></summary>


await sock.updateBlockStatus(jid, 'block') 
// or
await sock.updateBlockStatus(jid, 'unblock')

</details>
<details>
<summary><b>👁️ Privacy Settings (Last Seen, Profile, etc)</b></summary>


// Update Last Seen to 'nobody'
await sock.updateLastSeenPrivacy('none') 

// Update Profile Picture to 'contacts'
await sock.updateProfilePicturePrivacy('contacts')

// Update Read Receipts (Blue ticks)
await sock.updateReadReceiptsPrivacy('none')

</details>
🗑️ Chat Modification
<details>
<summary><b>🧹 Clear Chat</b></summary>


await sock.chatModify(
    {
        delete: true,
        lastMessages: [{ key: msg.key, messageTimestamp: msg.messageTimestamp }]
    },
    jid
)

</details>
<details>
<summary><b>🗑️ Delete Message (For Everyone)</b></summary>


await sock.sendMessage(jid, { 
    delete: key // The key of the message you want to delete
})

</details>
<details>
<summary><b>📦 Archive Chat</b></summary>


await sock.chatModify({ archive: true, lastMessages: [lastMsg] }, jid)

</details>
<div align="center">
Contact & Support
<p>Made with ❤️ by <strong>Vyzen Sockets</strong></p>
</div>

