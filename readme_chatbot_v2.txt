from chatterbot import ChatBot
from chatterbot.trainers import ListTrainer
from tkinter import *
import pyttsx3 as pp
import timeit
import speech_recognition as s
import threading

#Audio engine

engine=pp.init()

voices=engine.getProperty('voices')
print(voices)

engine.setProperty('voice',voices[0].id)

def speak(word):
    engine.say(word)
    engine.runAndWait()

time_funct=timeit.default_timer()    
#BOT Conversation    
bot=ChatBot("My Bot")

convo=[
    'Hello',
    'Hi there!',
    'My name is Bot,I was created by Team Cookie Catalyst',
    'How are you?',
    'I am doing great these days',
    'Thankyou',
    'In which city you live?',
    'I live in Lucknow',
    'In which langauges you talk?',
    'I mostly talk in English'
]


trainer=ListTrainer(bot)

#training bot with help of trainer

trainer.train(convo)

#answer=bot.get_response("What is your name?")
#print(answer)

#Code for GUI
main=Tk()                                                                 tk class ka object and custimize size
main.geometry("500x650")
main.title("BOT")

#take query:it takes audio as input and converts into string
def takequery():
    sr=s.Recognizer()
    sr.pause_threshold=1
    print("I am listening....")
    with s.Microphone() as m:
        try:
            audio=sr.listen(m)
            query=sr.recognize_google(audio,language='eng-in')
            print(query)
            textF.delete(0,END)
            textF.insert(0,query)
            ask_from_bot()
        except Exception as e:
            print("NOt Recognized")
    
def ask_from_bot():
 query=textF.get()                                                        right and assign t this variable name query  represent in prper format
 answer_from_bot= bot.get_response(query)                                 
 msgs.insert(END,"You:"+ query)                                          
 msgs.insert(END,"bot:"+str(answer_from_bot))
 speak(answer_from_bot)
 textF.delete(0,END)
 msgs.yview(END)

frame=Frame(main)

sc=Scrollbar(frame)
msgs=Listbox(frame,width=80,height=20,yscrollcommand=sc.set)

sc.pack(side=RIGHT ,fill=Y)

msgs.pack(side=LEFT,fill=BOTH,pady=10)

frame.pack()

#Creating text field.                                                        where we write the or ask the question

textF=Entry(main,font=("Verdana",20))

textF.pack(fill=X,pady=10)
btn=Button(main,text=">>>",font=("Verdana",20),command=ask_from_bot)
btn.pack()

# creating a function
def enter_function(event):
    btn.invoke()
    
#going to bind main window with enter key...
    main.bind('<Return>',enter_function)
    
def repeat():
    while True:
        takequery()
        
        
        
t=threading.Thread(target=repeat)
 
t.start()
main.mainloop()
