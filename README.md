<h1 align="center"> Cowin Vaccine Slot Booking Script</h1>

<div align="center">

<i>Below is a fork over the neat [covid-vaccine-booking](https://github.com/pallupz/covid-vaccine-booking). :metal:</i>



</div>

### 1. On your laptop
1. Make sure [Python 3.8+](https://python.org) is installed.
2. Clone this repo: ```git clone https://github.com/bombardier-gif/covid-vaccine-booking.git``` or download zip file and extract
3. Install the requirements:
	- ```pip install -r requirements.txt``` on Windows
	- ```pip3 install -r requirements.txt``` on Linux or Mac
4. Run the script:
	- ```python src\covid-vaccine-slot-booking.py``` on Windows
	- ```python3 src/covid-vaccine-slot-booking.py``` on Linux or Mac


<br>


## Before you start
1. If you face any issues please refer to the [troubleshooting section](#troubleshooting-common-problems) at the end of this doc
2. The captcha is a bit buggy and you may be required to make 5-6 tries before you are able to book
3. If you are still facing errors and want to run this script on windows using exe, please see the section below [How to run on windows](#how-to-run-on-windows)
4. Instructions for iOS have also been added. See the [Setup Guide for iOS](#setup-guide-for-ios) for details. Please note that its not possible to automate the OTP auto read on iOS completely, however its possible to make it a 1 tap process, which is far better than seeing and entering the OTP manually.

## What this repository does
1. Automates OTP read from the SMS after the token expires.
2. Randomly chooses one of the available slots instead of waiting for input from the user.


## COVID-19 Vaccination Slot Booking Script

This very basic CLI based script can be used to automate covid vaccination slot booking on Co-WIN Platform. 

### Important: 
- POC project. **Use at your own risk**.
- Do NOT use unless all beneficiaries selected are supposed to get the same vaccine and dose. 
- No option to register new user or add beneficiaries. This can be used only after beneficiary has been added through the official app/site
- If you accidentally book a slot, don't worry. You can always login to the official portal and cancel that.
- API Details: https://apisetu.gov.in/public/marketplace/api/cowin/cowinapi-v2
- And finally, I know code quality probably isn't great. Suggestions are welcome.


### Usage:

```
python src\covid-vaccine-slot-booking.py
```
If you're on Linux, install the beep package before running the Python script. To install beep, run:
```
sudo apt-get install beep
```
If you already have a bearer token, you can also use:
```
python src\covid-vaccine-slot-booking.py --token=YOUR-TOKEN-HERE
```

### Third-Party Package Dependency:
- ```tabulate``` : For displaying data in tabular format.
- ```requests``` : For making GET and POST requests to the API.
- ```inputimeout``` : For creating an input with timeout.

Install all dependencies by running:
```pip install -r requirements.txt```

<br>

## Steps:
1. Run script:
	```python src\covid-vaccine-slot-booking.py```
2. Select Beneficiaries. Read the important notes. You can select multiple beneficiaries by providing comma-separated index values such as ```1,2```:
	```
	Enter the registered mobile number: ██████████
	Requesting OTP with mobile number ██████████..  
	Enter OTP: 999999  
	Validating OTP..  
	Token Generated: █████████████████████████████████████████████████████████████  
	Fetching registered beneficiaries..  
	+-------+----------------------------+---------------------------+------------+  
	| idx   | beneficiary_reference_id   | name                      | vaccine    |  
	+=======+============================+===========================+============+  
	| 1     | ██████████████             | █████████████████████████ | COVISHIELD |  
	+-------+----------------------------+---------------------------+------------+  
	| 2     | ██████████████             | █████████████████         |            |  
	+-------+----------------------------+---------------------------+------------+  
	  
	################# IMPORTANT NOTES #################  
	# 1. While selecting beneficiaries, make sure that selected beneficiaries are all taking the same dose: either first OR second.  
	# Please do no try to club together booking for first dose for one beneficiary and second dose for another beneficiary.  
	#  
	# 2. While selecting beneficiaries, also make sure that beneficiaries selected for second dose are all taking the same vaccine: COVISHIELD OR COVAXIN.  
	# Please do no try to club together booking for beneficiary taking COVISHIELD with beneficiary taking COVAXIN.  
	###################################################  
	  
	Enter comma separated index numbers of beneficiaries to book for : 2
	```


3. Ensure correct beneficiaries are getting selected:
	```
	Selected beneficiaries:  
	+-------+----------------------------+-----------+  
	| idx   | beneficiary_reference_id   | vaccine   |  
	+=======+============================+===========+  
	| 1     | ██████████████             |           |  
	+-------+----------------------------+-----------+
	```

4. Select a state
	```
	+-------+-----------------------------+  
	| idx   | state                       |  
	+=======+=============================+  
	| 1     | Andaman and Nicobar Islands |  
	+-------+-----------------------------+  
	| 2     | Andhra Pradesh              |  
	+-------+-----------------------------+
	+-------+-----------------------------+
	+-------+-----------------------------+  
	| 35    | Uttar Pradesh               |  
	+-------+-----------------------------+  
	| 36    | Uttarakhand                 |  
	+-------+-----------------------------+  
	| 37    | West Bengal                 |  
	+-------+-----------------------------+
	```
	```
	Enter State index: 18
	```
5. Select districts you are interested in. Multiple districts can be selected by providing comma-separated index values
	```
	+-------+--------------------+  
	| idx   | district           |  
	+=======+====================+  
	| 1     | Alappuzha          |  
	+-------+--------------------+  
	| 2     | Ernakulam          |  
	+-------+--------------------+  
	| 3     | Idukki             |  
	+-------+--------------------+
	+-------+--------------------+
	+-------+--------------------+  
	| 13    | Thrissur           |  
	+-------+--------------------+  
	| 14    | Wayanad            |  
	+-------+--------------------+
	```
	```
	Enter comma separated index numbers of districts to monitor : 2,13
	```
6. Ensure correct districts are getting selected.
	```
	Selected districts:  
	+-------+---------------+-----------------+-----------------------+  
	| idx   | district_id   | district_name   | district_alert_freq   |  
	+=======+===============+=================+=======================+  
	| 1     | 307           | Ernakulam       | 660                   |  
	+-------+---------------+-----------------+-----------------------+  
	| 2     | 303           | Thrissur        | 3080                  |  
	+-------+---------------+-----------------+-----------------------+
	```
7. Enter the minimum number of slots to be available at the center:
	```
	Filter out centers with availability less than: 5
	```
8. Script will now start to monitor slots in these districts every 15 seconds. `Note`: It will ask you monitor frequency `ProTip`: Do not select less than 5 seconds it will bombard cowin server and will get your request blocked, create issues in OTP generation for your number. #85
	```
	===================================================================================  
	Centers available in Ernakulam from 01-05-2021 as of 2021-04-30 15:13:44: 0  
	Centers available in Thrissur from 01-05-2021 as of 2021-04-30 15:13:44: 0  
	No viable options. Waiting for next update in 15s.
	===================================================================================  
	Centers available in Ernakulam from 01-05-2021 as of 2021-04-30 15:13:59: 0  
	Centers available in Thrissur from 01-05-2021 as of 2021-04-30 15:13:59: 0  
	No viable options. Waiting for next update in 15s.
	```
9. If at any stage your token becomes invalid, the script will make a beep and prompt for ```y``` or ```n```. If you'd like to continue, provide ```y``` and proceed to allow using same mobile number
	```
	Token is INVALID.  
	Try for a new Token? (y/n): y
	Try for OTP with mobile number ███████████? (y/n) : y
	Enter OTP: 888888
	```
11. When a center with more than minimum number of slots is available, the script will make a beep sound - different frequency for different district. It will then display the available options as table:
	```
	===================================================================================  
	Centers available in Ernakulam from 01-05-2021 as of 2021-04-30 15:34:19: 1  
	Centers available in Thrissur from 01-05-2021 as of 2021-04-30 15:34:19: 0  
	+-------+----------------+------------+-------------+------------+------------------------------------------------------------------------------+  
	| idx   | name           | district   | available   | date       | slots                                                                        |  
	+=======+================+============+=============+============+==============================================================================+  
	| 1     | Ayyampilly PHC | Ernakulam  | 30          | 01-05-2021 | ['09:00AM-10:00AM', '10:00AM-11:00AM', '11:00AM-12:00PM', '12:00PM-02:00PM'] |  
	+-------+----------------+------------+-------------+------------+------------------------------------------------------------------------------+  
	---------->  Wait 10 seconds for updated options OR  
	---------->  Enter a choice e.g: 1.4 for (1st center 4th slot): 1.3
	```
12. Before the next update, you'll have 10 seconds to provide a choice in the format ```centerIndex.slotIndex``` eg: The input```1.4``` will select the vaccination center in second row and its fourth slot.

<br>

<br>

## Troubleshooting common problems

### Problem 1

   ```
   Can't setFont(Times-Roman) missing the T1 files?
   Originally <class 'TypeError'>: makeT1Font() argument 2 must be str, not None
   ```

   **Solution 1:** Then run the python script directly in the **src** folder after installing the required modules from **requirements.txt.** That solved it for me

   **Solution 2:** If you are running ubantu(tested) or Windows, this problem is due to some font files from package (reportlab) which are included in arch linux but not on Ubuntu. Follow these steps to install reportlab correctly. It can be done after you have installed all the requirements rom txt file.

   1. ```git clone https://github.com/Distrotech/reportlab.git```
   2. ```cd reportlab```
   3. ```python3 setup.py install```

     This will download all the font files. 

   **Solution 3**: Try to perform the test first in **test** folder for captcha first to see if this error still there.

 

    
### Problem 2

   Regarding beep package - Device not found or beep not found

   **Solution** : Follow these steps for Ubuntu

   1. ```sudo apt-get install beep``` #Install this once
   2. ```sudo modeprobe pcspkr``` #This will solve Device not found error 
   3. Testing beep is simple just type ```beep``` in terminal, this will produce a beep sound from your speakers.

