# First Aid Chatbot

A first-aid guidance chatbot (English only) that works with **text, photos and voice**. It has a web page (Gradio) and a REST API, both served through an ngrok address.

> **Not medical advice.** This is a learning project. In a life-threatening emergency, call your local emergency number first (Egypt: ambulance 123, police 122, fire 180, or 112).

## How it works

1. **Red-flag check**: emergency words (not breathing, chest pain, severe bleeding...) trigger a "call emergency services" banner first.
2. **Topic matching**: your trained classifier (TF-IDF + Logistic Regression), then a meaning search with sentence embeddings, then keyword backup.
3. **Answer**: Gemini writes short step-by-step instructions using only your data. If Gemini is off or fails, the bot shows the instructions straight from your CSV.
4. **Photo**: Gemini looks at the picture and picks the matching topic.
5. **Voice**: Gemini transcribes the recording (Whisper is the backup). Replies can be read aloud.

## Run it (Google Colab)

1. Open `first_aid_chatbot.ipynb` in Google Colab (**File > Open notebook > GitHub**).
2. Add your secrets (see below).
3. Run the cells in order, 1 to 8. After Cell 1, do **Runtime > Restart session**, then continue from Cell 2.
4. Cell 8 prints the web address and API address.

## Secrets

Nothing private is stored in the code. Save these in Colab: **left sidebar > key icon (Secrets) > Add new secret**, and turn **Notebook access ON** for each.

| Secret name | Required | What it is |
|---|---|---|
| `GEMINI_API_KEY` | For photos and voice | Free key from https://aistudio.google.com/apikey |
| `NGROK_AUTHTOKEN` | Yes | From https://dashboard.ngrok.com > Your Authtoken |
| `CHATBOT_PASSWORD` | Yes | Password for the web page (login user is `firstaid`) |
| `CHATBOT_API_KEY` | Yes | Long random text; sent by apps in the `X-API-Key` header |
| `NGROK_DOMAIN` | Optional | Your fixed ngrok domain, e.g. `your-name.ngrok-free.dev`. If empty, ngrok gives a random address |
| `DATA_CSV_URL` | Optional | Direct download link of your CSV. If empty, Cell 2 asks you to upload `first_aid_clean.csv` |
| `MODEL_URL` | Optional | Direct download link of your trained `.pkl`. If empty, Cell 2B asks you to upload it |

If a required secret is missing, the cell asks you to type it in a hidden box instead.

## Your data files

The CSV needs three columns: `text` (an example situation), `label` (the topic), `response` (the instructions for that topic).

The trained model file is a pickle (`.pkl`) saved with scikit-learn 1.6.1. **Only load model files you made yourself**, because a pickle from a stranger can run code on your machine.

## API

```
GET  /api/health
POST /api/ask    header: X-API-Key   body: {"text": "my friend is choking", "history": []}
POST /api/chat   header: X-API-Key   form: text, history (JSON list), image (file), audio (file)
```

## Privacy notes

- Voice recordings are saved on the Colab disk in `firstaid_data/recordings`. Delete them when you finish testing.
- Photos, voice and text you send are processed by Google's Gemini API when AI mode is on.
- Don't paste keys or tokens into notebook cells, and clear notebook outputs before committing.
