import random
import re
import time
from collections import defaultdict

# Importiamo le librerie necessarie per l'elaborazione del linguaggio naturale
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import WordNetLemmatizer

# Scarichiamo le risorse necessarie per NLTK
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')

# Inizializziamo il lemmatizzatore e la lista di stopwords
lemmatizer = WordNetLemmatizer()
stop_words = set(stopwords.words('english'))

# Le sessioni di mindfulness e le risposte del chatbot rimangono le stesse del programma precedente
# ...

# Creiamo un dizionario per memorizzare le frasi dell'utente e le risposte del bot
conversation_memory = defaultdict(list)

def preprocess_text(text):
    # Tokenizza, rimuovi le stopwords e lemmatizza il testo
    tokens = word_tokenize(text.lower())
    tokens = [lemmatizer.lemmatize(token) for token in tokens if token.isalnum()]
    tokens = [token for token in tokens if token not in stop_words]
    return tokens

def generate_response(user_input):
    # Preprocessa l'input dell'utente
    tokens = preprocess_text(user_input)
    
    # Trova la categoria più rilevante
    sentiment = analyze_sentiment(user_input)
    
    # Genera una risposta basata sulla categoria e sulle parole chiave
    if sentiment in chatbot_responses:
        base_response = random.choice(chatbot_responses[sentiment])
    else:
        base_response = random.choice(chatbot_responses["default"])
    
    # Personalizza la risposta incorporando alcune parole chiave dell'utente
    for token in tokens:
        if token in conversation_memory:
            base_response += f" Ricordo che hai menzionato '{token}' prima. Come si collega a ciò di cui stiamo parlando ora?"
    
    # Aggiungi una domanda di follow-up casuale
    follow_up_questions = [
        "Come ti fa sentire questo?",
        "Puoi dirmi di più al riguardo?",
        "Cosa pensi di fare a questo proposito?",
        "Come questo influenza la tua vita quotidiana?",
        "Hai parlato di questo con qualcun altro?"
    ]
    base_response += " " + random.choice(follow_up_questions)
    
    # Memorizza le parole chiave dell'utente per future conversazioni
    for token in tokens:
        conversation_memory[token].append(user_input)
    
    return base_response

def chatbot():
    print("Ciao! Sono un'AI progettata per supportarti e ascoltarti. Come ti senti oggi?")
    while True:
        user_input = input("Tu: ").lower()
        if user_input == "exit":
            print("AI: È stato un piacere parlare con te. Ricorda che sei forte e capace. Torna quando vuoi!")
            break
        
        response = generate_response(user_input)
        print("AI:", response)

# Il resto del programma (main, funzioni di mindfulness, etc.) rimane lo stesso
# ...

if __name__ == "__main__":
    main()
