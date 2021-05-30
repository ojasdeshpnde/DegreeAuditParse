from bs4 import BeautifulSoup
import requests
from tkinter import *
from tkinter import filedialog
import lxml
import os

base = Tk()
base.geometry('150x40')


#Before running get the following libraries:
# requests
# lxml
# os
# BeautifulSoup

# Also before running, make sure you go to pycharm console and type the following command:
# pip install future
# This will let you use the tkinter library


#Given a degree audit file in html and the set of Classes you want, you will get a set returned which will include all the classes you want
#If you want classes preivously taken: use time = "TAKEN"
#If you want classes in progress: use time = "IP"
#If you want classes you have scheduled: use time = "FUTURE"
def getCourseSet(fileK, time):

    soup = BeautifulSoup(fileK, "lxml")


    setOfCoursesIP = set()
    setOfCoursesFuture = set()
    setOfCoursesTaken = set()
    setOfCoursesRemaining = set()

    currentTerm = "AU20"

    if(time != "REMAINING"):
        trList = soup.find_all('tr', {'class': "takenCourse"})
        for setTr in trList:
            list1 = setTr.findChildren()
            if (termComparator(currentTerm, list1[0].text.strip()) == "="):
                addToSet(setOfCoursesIP, list1[1].text.strip())
            elif (termComparator(currentTerm, list1[0].text.strip()) == "<"):
                addToSet(setOfCoursesFuture, list1[1].text.strip())
            else:
                addToSet(setOfCoursesTaken, list1[1].text.strip())
        if(time == "TAKEN"):
            return setOfCoursesTaken
        elif(time == "IP"):
            return setOfCoursesIP
        elif(time == "FUTURE"):
            return setOfCoursesFuture
        else:
            raise Exception("This is an invalid set of Class Selection: Choose from TAKEN, IP or FUTURE")
    else:
        trList = soup.find_all('td',{'class':"fromcourselist"})
        for setTr in trList:
            list1 = setTr.findChildren()
            for x in list1:
                #print(x['class'][0])
                if(x['class'][0]!="number"):
                    string = x['department'] + " " + x['number']
                    addToSet(setOfCoursesRemaining,string)
        return setOfCoursesRemaining





#term 1 should generally be current term
#returns > if term1 > term2
# < if term 1 < term 2
# = if term 1 = term 2
def termComparator(term1, term2):
    temp1 = term1[2:len(term1)]
    temp2 = term2[2:len(term2)]
    if(temp1 == temp2):
        if(term1[0:2]=="AU" and term2[0:2]=="SP"):
            return ">"
        elif(term1[0:2]=="SP" and term2[0:2]=="AU"):
            return "<"
        else:
            return "="
    elif(temp1 > temp2):
        return ">"
    elif(temp1 < temp2):
        return "<"

#given a setA and course, adds course if it hasn't already been added to setA
def addToSet(setA,course):
    bol = True
    for k in setA:
        if(k==course):
            bol = False
    if(bol == True):
        setA.add(course)
    return

# MAIN Method below: Above are methods

#with filedialog.askopenfile(initialdir="/") as input:
#    fileK = open(input.name)
fileK = open("DegAudit.html")
l = getCourseSet(fileK,"REMAINING")

for k in l:
    print(k)
