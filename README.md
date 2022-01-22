# from asyncore import file_dispatcher
# from email.mime import image
# from statistics import mode
import cv2
import time
import random
import dropbox

start_time = time.time()

def take_snapshot():
    number = random.randint(0,100)
    videocaptureobject = cv2.VideoCapture(0)

    result = True
    while(result):
        ret,Frame = videocaptureobject.read()
        image_name = "img"+str(number)+".png"
        cv2.imwrite(image_name,Frame)

        start_time = time.time
        result = False
        print("Snaphot_Taken")
        return image_name
        

    videocaptureobject.release()
    cv2.destroyAllWindows()

def upload_file(image_name):
    access_token = ""
    file = image_name
    file_from = file
    file_to = "/testfolder/"+(image_name)
    dbx = dropbox.Dropbox(access_token)
    with open(file_from,'rb')as f:
        dbx.files_upload(f.read(),file_to,mode=dropbox.files.WriteMode.overwrite)
        print("file_uploaded")

            

def main():
    while (True):
        if((time.time()-start_time)>=5):
            name=take_snapshot()
            upload_file(name)

main()
