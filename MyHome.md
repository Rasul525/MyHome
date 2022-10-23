# MyHome
import telebot
import time
#import pyfirmata
#from pyfirmata import util

PassCode = 'hello'
Token = '5401128449:AAHQhtCWe10yG6KIcA2fjrvSRF8ZvErSmAI'
Home = telebot.TeleBot(Token)
#Controler = pyfirmata.ArduinoMega('COM4')


#Pinlerin Teyini
#LightsPin = Controler.digital[13] #+
#GasSensPin = Controler.analog[0] #+
#DoorOpenPin = Controller.digital[12] #+
#DoorClosePin = Controller.digital[11] #+
#FanPin = Controller.digital[10] #+



@Home.message_handler(commands=['LightsOn'])
def LightsOn(message):
    #LightsPin.write(1)
    Home.reply_to(message,'Isiqlar Yandirildi!')
    print('Isiq yandi!')

@Home.message_handler(commands=['DoorOpen'])
def DoorOpen(message):
   while True:
        #DoorOpenPin.write(1)
        time.sleep(2)
        #DoorOpenPin.write(0)
        time.sleep(2)
        #DoorOpenPin.write(0)

@Home.message_handler(commands=['DoorClose'])
def DoorOpen(message):
   while True:
        #DoorClosePin.write(1)
        time.sleep(2)
        #DoorClosePin.write(0)
        time.sleep(2)
        #DoorClosePin.write(0)

#FanOn
@Home.message_handler(commands=['FanOn'])
def DoorOpen(message):
    #FanPin.write(1)

#FanOff
@Home.message_handler(commands=['FanOff'])
def DoorOpen(message):
    #FanPin.write(0)

@Home.message_handler(commands=['LightsOff'])
def LightsOff(message):
    #LightsPin.write(0)
    Home.reply_to(message,'Isiqlar Sonduruldu!')
    print('Isiqlar sondu!')

@Home.message_handler(commands=['GasRead'])
def GasRead(message):
    print('Qaz sensoru oxundu!')
    #reader = pyfirmata.util.Iterator(Controler)
    #reader.start()
    #sensor = GasSensPin
    #sensor.enable_reporting()
    while True:
    #    data = str(sensor.read())
     #   if data != 'None':
      #      print(sensor.read())
           # Home.reply_to(message,"Havadaki qaz miqdarı: {}".format(data))
            print('Melumat gonderildi!')
            break

@Home.message_handler(commands=['clear'])
def StartCommand(message):
    Hosts = open('C:\MyHome\Hosts.txt', 'w')
    print('Siyahi ve fayl temizlendi!')

@Home.message_handler(commands=['start'])
def StartCommand(message):
    Home.reply_to(message, 'Ev sahibləri üçün giriş:\n /MyHome\nYeni gələnlər üçün giriş:\n /Register')


@Home.message_handler(commands=['Register'])
def StartCommand(message):
    Home.reply_to(message, 'Evinizə xoş gəldiniz! Parolu daxil edin.')
    @Home.message_handler(func=lambda messaj: True)
    def IDCheck(message):
        UserInput = message.text
        UserID = message.chat.id
        print(UserInput)
        print(UserID)


        if str(UserInput).lower() == str(PassCode):
            Hosts = open('C:\MyHome\Hosts.txt', 'a')
            Hosts.writelines(str(UserID))
            Hosts.writelines(' ')

            HostList = []
            Hosts = open('C:\MyHome\Hosts.txt', 'r')
            HostList.append(Hosts.read())
            print(HostList)
            for i in HostList:
                HostList2 = HostList[0].split(' ')
                print(HostList2)
                if str(UserID) in HostList2:
                    print('Yeni sahibin qeydiyyati tamamlandi!')
                    Home.reply_to(message,"Ev sahibinin tanınması başa çatdı! \n Əmrlər menüsu: \n /Info \n /UserGuide \n /LightsOn \n /LightsOff \n /OpenDoor \n /CloseDoor \n /GasRead \n /DoorCheck  \n /WindowCheck")
                else:
                    print('New user!')
                    Home.reply_to(message, 'You are a new user!')
        elif str(UserInput) != str(PassCode):
            print("Uğursuz giriş cəhdi!")
            Home.reply_to(message, 'Şifrənizi daxil edin.')



@Home.message_handler(commands=['MyHome'])
def StartCommand(message):
    Home.reply_to(message, 'Evinizə xoş gəldiniz! Adinizi daxil edin.')

    @Home.message_handler(func=lambda m: True)
    def IDCheck(message):
        UserInput = message.text
        UserID = message.chat.id
        print(UserInput)
        print(UserID)

        HostList = []
        Hosts = open('C:\MyHome\Hosts.txt', 'r')
        HostList.append(Hosts.read())
        print(HostList)
        for i in HostList:
            HostList2 = HostList[0].split(' ')
            print(HostList2)
            if str(UserID) in HostList2:
                print('Ev sahibin taninmasi basa catdi!')
                Home.reply_to(message,"Ev sahibinin tanınması başa çatdı! \n Əmrlər menüsu: \n /Info \n /UserGuide \n /LightsOn \n /LightsOff \n /OpenDoor \n /CloseDoor \n /GasRead \n /DoorCheck  \n /WindowCheck")
            elif str(UserID) not in HostList2:
                Home.reply_to(message,'Siz ev sahibi deyilsiniz! \nQeydiyyatdan keçmək üçün /register əmrindən istifadə edin.')


while True:
    Home.polling()
