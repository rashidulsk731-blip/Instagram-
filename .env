const express = require("express");
const multer = require("multer");
const path = require("path");
const fs = require("fs");
require("dotenv").config();

const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

const uploadDir = path.join(__dirname, "uploads");

if (!fs.existsSync(uploadDir)) {
  fs.mkdirSync(uploadDir);
}

const storage = multer.diskStorage({
  destination: uploadDir,
  filename: (req, file, cb) => {
    const name =
      Date.now() +
      "-" +
      file.originalname.replace(/[^a-zA-Z0-9._-]/g, "_");

    cb(null, name);
  }
});

const upload = multer({ storage });

app.use("/uploads", express.static(uploadDir));
app.use(express.static("public"));

/* =========================
   SETTINGS
========================= */

let settings = {
  keyword: "FILE",
  message:
    "ধন্যবাদ! আপুনি বিচৰা File-টো তলৰ Link-ৰ পৰা Download কৰিব পাৰিব:",
  fileUrl: ""
};

/* =========================
   ADMIN SETTINGS
========================= */

app.get("/api/settings", (req, res) => {
  res.json(settings);
});

app.post("/api/settings", (req, res) => {
  settings.keyword =
    String(req.body.keyword || "FILE").trim().toUpperCase();

  settings.message =
    String(req.body.message || "").trim();

  res.json({
    success: true,
    settings
  });
});

/* =========================
   FILE UPLOAD
========================= */

app.post("/api/upload", upload.single("file"), (req, res) => {
  if (!req.file) {
    return res.status(400).json({
      success: false,
      message: "File select কৰক।"
    });
  }

  settings.fileUrl =
    `${process.env.PUBLIC_URL}/uploads/${req.file.filename}`;

  res.json({
    success: true,
    file: req.file.originalname,
    url: settings.fileUrl
  });
});

/* =========================
   INSTAGRAM WEBHOOK VERIFY
========================= */

app.get("/webhook", (req, res) => {
  const mode = req.query["hub.mode"];
  const token = req.query["hub.verify_token"];
  const challenge = req.query["hub.challenge"];

  if (
    mode === "subscribe" &&
    token === process.env.VERIFY_TOKEN
  ) {
    return res.status(200).send(challenge);
  }

  res.sendStatus(403);
});

/* =========================
   INSTAGRAM WEBHOOK
========================= */

app.post("/webhook", async (req, res) => {
  try {
    const body = req.body;

    console.log(
      "Instagram Webhook:",
      JSON.stringify(body, null, 2)
    );

    /*
      Meta-ই পঠোৱা Instagram event
      ইয়াত comment/message event process কৰিবা।
    */

    if (body.object !== "instagram") {
      return res.sendStatus(404);
    }

    /*
      Production-ত ইয়াত:
      1. Comment text check
      2. Keyword match
      3. Commenter's Instagram ID
      4. Instagram Messaging API
      5. Automatic DM
      কৰিব লাগিব।
    */

    res.sendStatus(200);

  } catch (error) {
    console.error(error);
    res.sendStatus(500);
  }
});

/* =========================
   TEST AUTO-DM FUNCTION
========================= */

app.post("/api/test", async (req, res) => {
  const comment = String(req.body.comment || "")
    .trim()
    .toUpperCase();

  if (comment !== settings.keyword) {
    return res.json({
      success: false,
      message: "Keyword match হোৱা নাই।"
    });
  }

  if (!settings.fileUrl) {
    return res.json({
      success: false,
      message: "আগতে File upload কৰক।"
    });
  }

  const dmText =
    `${settings.message}\n\n${settings.fileUrl}`;

  console.log("DM MESSAGE:");
  console.log(dmText);

  res.json({
    success: true,
    message: dmText
  });
});

/* =========================
   SERVER
========================= */

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`RA STUDIO Server running on port ${PORT}`);
});
