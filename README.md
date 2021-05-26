# Cobot
Vaccination is now open for people of age above 18. But getting a vaccination appointment is as difficult as finding a needle in a haystack because slots are filled in an eye-blink. So, searching through the Co-WIN portal manually is slow and sometimes irritating as well because you will almost all the time land with a page showing all slots booked as there is a huge crowd of people who want to get vaccinated as early as possible.

So, this problem can be solved by making it easier for people to find out when the next appointment is available in their region. We will be using Co-WIN API which is made public by the government and we will also use telegram BOT API to send automated alerts on telegram about the details of the vaccination center.

So, let us get started!
You can get API from the API Setu website of government: https://apisetu.gov.in/public/api/cowin. These APIs are subject to a rate limit of 100 API calls per 5 minutes per IP. So, keep this in mind. We will be finding the slot for a week from today in a region by using a PIN CODE.  So, first let’s see what the output of API is if I give Pin code: 243001 and Date: 19-05-2021

URL: https://cdn-api.co-vin.in/api/v2/appointment/sessions/public/findByPin?pincode=243001&date=19-05-2021
Now, send the get request to URL with a date and pin code. The get request returns us data in JSON format and from this data, we need to filter out what we need. Since, we are planning to make alerts for 2 age groups i.e 18 to 44, above 45. So, we will make two separate lists for these two age groups to store vaccination center details.

The vaccination centre’s details include the following information:
1.Centre name
2.Centre Address
3. Vaccine Name
4. Available quantity for dose 1
5. Available quantity for dose 2

Now add each center’s detail in the list for the two age groups separately
After this code we get two lists: listOfAllCentresFor45 and listOfAllCentresFor18. These lists have details of all the centers where a person can go for vaccination.

So, here half the path is covered.

Now, we need to send these details on telegram. So, we will use telegram bot API and create a group to send these details as messages. Refer documentation of telegram bot API to know how to get this API.

Sending a list of centers vaccinating 45+ age group people:

Create a message string first out of the list so that this message string can directly be passed to

URL. Now you need to provide the API_KEY and CHAT_ID which you got from the telegram bot. Replace

these two in the base_url below to complete the URL.

This is a URL for those above age >= 45:
For this we will be using schedular module of python. We will schedule the above code to run daily at a specific time.

Example: schedular.every().day().at(“10:00”).do(function_name)

So, put the above full code under a function for eg. vaccine_notifier, then schedule it for three times a day:

base_url = “https://api.telegram.org/bot_YOUR-API_‘+’sendMessage?chat_id=_YOUR-CHAT-ID_&text={0}.format(messageFor45)
