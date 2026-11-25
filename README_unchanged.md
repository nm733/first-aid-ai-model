# First Aid Chatbot

A first-aid guidance chatbot (English only) that works with **text, photos and voice**. It has a web page (Gradio) and a REST API, both served through an ngrok address.

> **Not medical advice.** This is a learning project. In a life-threatening emergency, call your local emergency number first (Egypt: ambulance 123, police 122, fire 180, or 112).

> **Keep this repository PRIVATE.** The notebook contains the download links for the data and the trained model, and your fixed ngrok address.

## How it works

1. **Red-flag check**: emergency words (not breathing, chest pain, severe bleeding...) trigger a "call emergency services" banner first.
2. **Topic matching**: your trained classifier (TF-IDF + Logistic Regression), then a meaning search with sentence embeddings, then keyword backup.
3. **Answer**: Gemini writes short step-by-step instructions using only your data. If Gemini is off or fails, the bot shows the instructions straight from your CSV.
4. **Photo**: Gemini looks at the picture and picks the matching topic.
5. **Voice**: Gemini transcribes the recording (Whisper is the backup). Replies can be read aloud.

## Run it (Google Colab)

1. Open `first_aid_chatbot.ipynb` in Google Colab (**File > Open notebook > GitHub**).
2. Run the cells in order. After Cell 1, do **Runtime > Restart session**, then continue from Cell 2.
3. Cell 8 prints the web address and API address.

The data (CSV) and the trained model download automatically in Cells 2 and 2B.

## Keys and passwords

These are never stored in the code. Save them in Colab: **left sidebar > key icon (Secrets) > Add new secret**, with **Notebook access ON**. If a secret is missing, the cell asks you to type it in a hidden box.

| Secret name | What it is |
|---|---|
| `GEMINI_API_KEY` | Free key from https://aistudio.google.com/apikey (needed for photos and voice) |
| `NGROK_AUTHTOKEN` | From https://dashboard.ngrok.com > Your Authtoken |
| `CHATBOT_PASSWORD` | Password for the web page (login user is `firstaid`) |
| `CHATBOT_API_KEY` | Long random text; sent by apps in the `X-API-Key` header |

## API

```
GET  /api/health
POST /api/ask    header: X-API-Key   body: {"text": "my friend is choking", "history": []}
POST /api/chat   header: X-API-Key   form: text, history (JSON list), image (file), audio (file)
```

## Privacy notes

- Voice recordings are saved on the Colab disk in `firstaid_data/recordings`. Delete them when you finish testing.
- Photos, voice and text you send are processed by Google's Gemini API when AI mode is on.
- Only load model files you made yourself: a `.pkl` file from a stranger can run code.
- Don't paste keys or tokens into notebook cells, and clear notebook outputs before committing.
