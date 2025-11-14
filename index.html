const express = require('express');
const fs = require('fs');
const path = require('path');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

const DATA_FILE = path.join(__dirname, 'data.json');
const SCANS_FILE = path.join(__dirname, 'scans.json');
const MSG_FILE = path.join(__dirname, 'messages.json');

function readJson(file, fallback) {
  try {
    return JSON.parse(fs.readFileSync(file, 'utf8') || '');
  } catch (e) {
    return fallback;
  }
}
function writeJson(file, obj) {
  fs.writeFileSync(file, JSON.stringify(obj, null, 2));
}

// Ensure files exist
if (!fs.existsSync(DATA_FILE)) writeJson(DATA_FILE, {
  "example123": {
    "title": "Jonas — Pamestų raktų savininkas",
    "description": "Jei radote šį daiktą, prašau susisiekite ir susitarsim dėl grąžinimo.",
    "contact": { "email": "jonas@example.com", "phone": "+37060000000", "showEmail": false },
    "ownerMessage": "Ačiū, kad grąžinote!"
  }
});
if (!fs.existsSync(SCANS_FILE)) writeJson(SCANS_FILE, []);
if (!fs.existsSync(MSG_FILE)) writeJson(MSG_FILE, []);

// Simple helper to mask email for privacy if showEmail=false
function maskEmail(email) {
  if (!email) return '';
  const parts = email.split('@');
  if (parts.length !== 2) return email;
  const name = parts[0];
  const domain = parts[1];
  if (name.length <= 2) return '****@' + domain;
  return name.substring(0, 1) + '***' + name.substring(name.length - 1) + '@' + domain;
}

// GET info page for a QR code
app.get('/i/:code', (req, res) => {
  const code = req.params.code;
  const data = readJson(DATA_FILE, {});
  const item = data[code];

  // log scan
  const scans = readJson(SCANS_FILE, []);
  scans.push({ code, timestamp: new Date().toISOString(), ua: req.headers['user-agent'] || '', ip: req.ip });
  writeJson(SCANS_FILE, scans);

  if (!item) {
    res.status(404).send(`\n<!doctype html>\n<html><head><meta charset="utf-8"><title>Not found</title></head><body><h1>Nepriskirtas QR kodas</h1><p>Toks QR kodas nerastas.</p></body></html>`);
    return;
  }

  // Build contact display based on owner preferences
  const showEmail = !!(item.contact && item.contact.showEmail);
  const emailDisplay = showEmail ? item.contact.email : maskEmail(item.contact.email);
  const phoneDisplay = item.contact && item.contact.phone ? item.contact.phone : '';

  // Simple HTML page - you can replace with a React app or template engine
  res.send(`\n<!doctype html>\n<html lang="lt">\n<head>\n  <meta charset="utf-8">\n  <meta name="viewport" content="width=device-width,initial-scale=1">\n  <title>${escapeHtml(item.title || 'Kontaktas')}</title>\n  <style>body{font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial;margin:20px;padding:0;color:#111} .card{max-width:640px;margin:0 auto;border-radius:12px;padding:18px;border:1px solid #e6e6e6;box-shadow:0 6px 24px rgba(0,0,0,0.04)} h1{margin-top:0} .contact{margin-top:12px} form{margin-top:18px} label{display:block;margin-bottom:6px} input,textarea{width:100%;padding:8px;border:1px solid #ccc;border-radius:6px} button{margin-top:8px;padding:10px 14px;border-radius:8px;border:0;background:#111;color:#fff} .muted{color:#666;font-size:0.9rem}</style>\n</head>\n<body>\n  <div class="card">\n    <h1>${escapeHtml(item.title)}</h1>\n    <p class="muted">${escapeHtml(item.description || '')}</p>\n    <div class="contact">\n      <strong>El. paštas:</strong> <span>${escapeHtml(emailDisplay)}</span><br/>\n      <strong>Telefonas:</strong> <span>${escapeHtml(phoneDisplay)}</span>\n    </div>\n\n    <hr/>\n    <p class="muted">Siųskite trumpą žinutę savininkui (neatskleidžiame pardavimo ar kitos asmeninės informacijos):</p>\n    <form method="POST" action="/i/${encodeURIComponent(code)}/message">\n      <label>Jūsų vardas (optional)<br><input name="name" maxlength="80"/></label>\n      <label>Žinutė savininkui<br><textarea name="message" required maxlength="1000" rows="4"></textarea></label>\n      <button type="submit">Siųsti žinutę</button>\n    </form>\n  </div>\n</body>\n</html>\n`);
});

// POST message from finder -> saved to messages.json and optionally emailed
app.post('/i/:code/message', (req, res) => {
  const code = req.params.code;
  const data = readJson(DATA_FILE, {});
  const item = data[code];
  if (!item) return res.status(404).send('QR kodas nerastas');

  const name = String(req.body.name || '').slice(0, 80);
  const message = String(req.body.message || '').slice(0, 1000);

  const messages = readJson(MSG_FILE, []);
  const entry = { code, timestamp: new Date().toISOString(), name, message, ua: req.headers['user-agent'] || '', ip: req.ip };
  messages.push(entry);
  writeJson(MSG_FILE, messages);

  // OPTIONAL: send email to owner if SMTP is configured (nodemailer) - uncomment/install if you want
  /*
  const nodemailer = require('nodemailer');
  // Configure transporter with env vars
  const transporter = nodemailer.createTransport({
    host: process.env.SMTP_HOST,
    port: Number(process.env.SMTP_PORT || 587),
    secure: process.env.SMTP_SECURE === 'true',
    auth: { user: process.env.SMTP_USER, pass: process.env.SMTP_PASS }
  });
  transporter.sendMail({
    from: 'no-reply@example.com',
    to: item.contact.email,
    subject: `Nauja žinutė dėl daikto ${code}`,
    text: `Vardas: ${name}\n\nŽinutė:\n${message}`
  }).catch(err => console.error('Mail error', err));
  */

  // Respond with a thank-you / owner message
  res.send(`<!doctype html><html><head><meta charset="utf-8"><title>Ačiū</title></head><body style="font-family:system-ui;padding:20px"><h1>Ačiū</h1><p>Žinutė išsaugota. ${escapeHtml(item.ownerMessage || '')}</p><p><a href="/i/${encodeURIComponent(code)}">Grįžti</a></p></body></html>`);
});

// Admin-ish endpoint: return JSON for code (for quick testing). In production protect with auth.
app.get('/admin/code/:code', (req, res) => {
  const code = req.params.code;
  const data = readJson(DATA_FILE, {});
  if (!data[code]) return res.status(404).json({ error: 'not found' });
  res.json(data[code]);
});

// Utility: escape HTML to avoid injection
function escapeHtml(str) {
  if (!str) return '';
  return String(str)
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`QR info app listening on port ${PORT}. Visit http://localhost:${PORT}/i/example123`));
