import random
from transformers import pipeline

# Laden von Modellen
emotion_classifier = pipeline("text-classification", model="j-hartmann/emotion-english-distilroberta-base")

# Simuliertes Gedächtnis für Emotionen
memory = {}

def detect_emotion(text):
    result = emotion_classifier(text)
    return result[0]['label']

def generate_emotional_response(emotion, user_input):
    responses = {
        "joy": ["Ich spüre deine Freude! Was macht dich so glücklich?", 
                "Es ist wundervoll, dass du so positiv bist!"],
        "sadness": ["Das tut mir leid zu hören. Kann ich dir helfen?", 
                    "Ich verstehe, das klingt wirklich traurig."],
        "anger": ["Bitte bleib ruhig, ich bin hier, um zu helfen.", 
                  "Ich merke, dass du wütend bist. Möchtest du darüber sprechen?"],
        "fear": ["Das klingt beängstigend. Was kann ich tun, um dir zu helfen?", 
                 "Das ist nicht leicht, ich bin für dich da."],
        "neutral": ["Erzähl mir mehr.", 
                    "Interessant. Was denkst du darüber?"]
    }
    # Dynamische Reaktion basierend auf Zufall
    return random.choice(responses.get(emotion, ["Ich bin mir nicht sicher, was ich sagen soll."]))

def remember_emotion(user_input, emotion):
    memory[user_input] = emotion
    if len(memory) > 10:  # Begrenzung des Gedächtnisses
        memory.pop(next(iter(memory)))

def main():
    print("Hallo! Ich bin deine emotionale KI. Erzähl mir, wie du dich fühlst.")
    while True:
        user_input = input("Du: ")
        if user_input.lower() in ["exit", "quit"]:
            print("Tschüss! Bleib positiv!")
            break
        emotion = detect_emotion(user_input)
        remember_emotion(user_input, emotion)
        response = generate_emotional_response(emotion, user_input)
        print(f"KI: {response}")

if __name__ == "__main__":
    main()
