# -*- coding: utf-8 -*-
"""
Created on Mon May 25 13:48:27 2020

@author: skygt
"""

# -*- coding: utf-8 -*-
"""
Created on Sat Mar 21 18:05:43 2020

@author: skygt
"""

# import edilen packge
from imutils.video import VideoStream
from pyzbar import pyzbar
import argparse
import datetime
import imutils
import time
import cv2
import sqlite3 as sql
import xlrd
import pandas as pd
import pymysql
import MySQLdb
import json 
import csv
import mysql.connector
from mysql.connector import Error
import pandas as pd

import sqlite3

from openpyxl import Workbook
import csv

ap = argparse.ArgumentParser()
ap.add_argument("-o", "--cikti", type=str, default="barkodlar.csv",
	help="barkod içeren CSV dosyasına çıkış yolu")
args = vars(ap.parse_args())

# video akşının başlatılması
print("[INFO] starting video stream...")
vs = VideoStream(src=0).start()
# vs = VideoStream(usePiCamera=True).start()
time.sleep(2.0)

# çıktı icin csv dosyasısı oluşturuyoz
csv = open(args["cikti"], "w")
found = set()



#video akışından kareler üzerinde döngü
while True:
	# çerçeveli video akışından kareyi yakalayın ve yeniden boyutlandırın
    #maksimum 400 piksel genişliğe sahipls
	frame = vs.read()
	frame = imutils.resize(frame, width=500)

	# çerçevedeki barkodları bulun ve barkodların her birinin kodunu çözüyoz
	barcodes = pyzbar.decode(frame)

    	# algılanan barkodların üzerinden döngü
	for barcode in barcodes:		
        #barkodun sınırlayıcı kutu konumunu çıkarın ve çizin
        #resimdeki barkodu çevreleyen sınırlayıcı kutu
		(x, y, w, h) = barcode.rect
		cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 0, 255), 2)

		#barkod verileri bir bayt nesnesidir, bu yüzden çizmek istiyorsak
		#çıktı görüntümüzde önce bir dizeye dönüştürmeliyiz
		barcodeData = barcode.data.decode("utf-8")
		barcodeType = barcode.type
        
	    #barkod verilerini ve barkod türünü resim üzerine çizin
		text = "{} ({})".format(barcodeData, barcodeType)
		cv2.putText(frame, text, (x, y - 10),
			cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 2)

		 q
        #barkod metni şu anda CSV dosyamızda değilse yazın
        #zaman damgası + barkodu diske koyun ve seti güncelleyin
		if barcodeData not in found:
			csv.write("{}\n".format(barcodeData))#csv yazip excell dosyası yapıyoruz saat tarih aliryoz
			csv.flush()
			found.add(barcodeData)
            
     #çıktı çerçevesini göster
	cv2.imshow("Barcode Scanner", frame)
	key = cv2.waitKey(1) & 0xFF
 
	
    #eğer q tuşuna basılırsa, döngüden kopar
	if key == ord("Q"):
		break


f=open("barkodlar.csv","r")
fstring=f.read();
#verileri string yapıp listeye atıyoruz
flist=[]
for line in fstring.split('\n'):
    flist.append(line.split(','))
print(flist)


csv.close
vs.stop








        


    
    