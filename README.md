# Chatter-bot-corpus

# Import necessary libraries
from chatterbot import ChatBot
from chatterbot.trainers import ListTrainer, ChatterBotCorpusTrainer

# Create the chatbot
bot = ChatBot("Chad")

# Train with custom Safe Computing topic lists
trainer = ListTrainer(bot)

passwords = [
    "What makes a good password?",
    "A good password is long, unique, and has a mix of characters.",
    "Should I reuse passwords?",
    "No, always use a different password for each account.",
    "How can I remember passwords?",
    "Use a password manager to store them securely."
]

phishing = [
    "What is phishing?",
    "Phishing is a trick to steal your information using fake messages.",
    "How do I spot phishing?",
    "Look for misspellings, strange links, and urgent requests.",
    "What do I do if I get a phishing email?",
    "Don't click anything, and report it to IT or delete it."
]

malware = [
    "What is malware?",
    "Malware is software that harms your device or steals data.",
    "How do I avoid malware?",
    "Don’t download unknown files or click shady links.",
    "Should I update my computer?",
    "Yes, updates patch security holes and keep you safe."
]

privacy = [
    "How do I protect my privacy online?",
    "Avoid oversharing personal info and check privacy settings.",
    "Should I share my location?",
    "Only if necessary — turn off location sharing when possible.",
    "Can websites track me?",
    "Yes, through cookies and trackers. Use private browsing."
]

# Train chatbot with the lists
trainer.train(passwords)
trainer.train(phishing)
trainer.train(malware)
trainer.train(privacy)

# Corpus training
corpus_trainer = ChatterBotCorpusTrainer(bot)
corpus_trainer.train(
    "chatterbot.corpus.english.conversations",
    "chatterbot.corpus.english.greetings",
    "chatterbot.corpus.english.computers",
    "chatterbot.corpus.english.ai",
    "chatterbot.corpus.english.science"  # You can swap with another corpus like 'technology'
)

# Welcome message
name = input("What is your name?: ")
print("Hello, " + name + "! I am Chad the Safe Computing bot.")
print("Ask me questions about passwords, phishing, malware, or privacy.")
print("If you like my answer, type 'good response' so I can learn!")
print("Say 'bye' when you're done.")

# Start conversation loop
previousInput = ""
botResponse = ""
second = False  # Tracks if we've had one full response

while True:
    userInput = input(name + ": ")

    if userInput.lower() == "good response":
        if second:
            bot.learn_response(botResponse, previousInput)
            print("Chad: Thanks, I'm learning!")
        else:
            print("Chad: We haven't started talking yet!")

    elif "bye" in userInput.lower():
        print("Chad: Goodbye! Stay safe online!")
        break

    else:
        botResponse = bot.get_response(userInput)
        print("Chad:", botResponse)
        previousInput = userInput
        second = True  # Now the bot can learn next time
