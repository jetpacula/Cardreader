# -*- coding: utf-8 -*-
"""
Created on Tue Feb 18 12:00:44 2020

@author: nmelniko2
"""
#in placeCard app recommended to use Z2USB decode format when reading card
import time
import os
import msvcrt
import sys

import datetime
import csv


if getattr(sys,'frozen',False): 
    application_path = os.path.dirname(sys.executable)
elif __file__:
    application_path = os.path.dirname(__file__)

def readInput( caption, timeout = 3): 
    i=0
    start_time = time.time()
    sys.stdout.write('%s:'%(caption))
    sys.stdout.flush()
    input = ''
    while True:
        if msvcrt.kbhit():#get input flow for the console
            byte_arr = msvcrt.getche()
            i=i+1
            
            if ord(byte_arr) == 13: # enter_key
                break
            elif ord(byte_arr) >= 32: #space_char
                input += "".join(map(chr,byte_arr))
            if i==5:
                break
        if (time.time() - start_time) > timeout and i==0: #while no chars at caret, increase timeout
            timeout+=2
        if i<6 and (time.time() - start_time) > timeout and i>0:#if timeout for swipe is over and less than string is less than required length
            return "invalid input"
            
            
    print('')  # needed to move to next line

    if len(input) > 0:
        return input
    elif i==0:
        return readInput('waiting for swipe',timeout=9999999)
path = application_path
outputfilename = 'dataout.tsv'
outputfile=os.path.join(application_path,'dataout.tsv')

full_date = str(datetime.datetime.now())


matchfile = os.path.join(application_path,'data.csv')
matchfile = open(matchfile, 'r') #opening file
print(matchfile)
d = {}
id = 0 #counter to skip header
for x in matchfile:

    if id ==0:
        id += 1
        continue
    else:
        t = x.split(";")#split csv lines
        d[t[0]] = t[1] #

while True:
    x = readInput('swipe card')
    if x in d:
        print(d[x],full_date)
        
        
        with open(outputfile,'a') as tsvoutput:
            
            writer = csv.writer(tsvoutput)
            print(','.join([''.join(x),''.join(d[x]),''.join(full_date)]))
            writer.writerow(['\t'.join([x,d[x],full_date])])
        continue
    else:
        print("no such employee")

