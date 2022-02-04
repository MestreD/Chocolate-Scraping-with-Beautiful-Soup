import codecademylib3_seaborn
from bs4 import BeautifulSoup
import requests
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

#Let’s make a request to this site to get the raw HTML
webpage = requests.get("https://content.codecademy.com/courses/beautifulsoup/cacao/index.html")

#Create a BeautifulSoup object to traverse this HTML
soup = BeautifulSoup(webpage.content, "html.parser")
print(soup)
#get all of the tags that contain the ratings
ratings_lst = soup.find_all(attrs={"class": "Rating"})
#Loop through the ratings tags and get the text contained in each one.
ratings = [float(val.get_text()) for val in ratings_lst[1:]]

#Create a histogram of the ratings values
plt.hist(ratings)
plt.title("Ratings")
plt.show()

#let’s find all the tags on the webpage that contain the company names.
companies = [company.get_text() for company in soup.select(".Company")[1:]]
#print(companies)

#Create a DataFrame 
df = pd.DataFrame({"Company":companies, "Rating":ratings})
#Get the 10 highest rated chocolate companies
mean_rating = df.groupby("Company").Rating.mean()
ten_best=mean_rating.nlargest(10)
print(ten_best)

#create a list that contains all of the cocoa percentages.
cocoa_percentages = [int(float(company.get_text().strip("%"))) for company in soup.select(".CocoaPercent")[1:]]
#Add the cocoa percentages as a column in the DataFrame
df["CocoaPercentage"]= cocoa_percentages
#print(df)
#Make a scatterplot of ratings vs percentage of cocoa
plt.scatter(df["CocoaPercentage"], df["Rating"], color="g")
plt.title("Rating vs Cocoa %")
z = np.polyfit(df.CocoaPercentage, df.Rating, 1)
line_function = np.poly1d(z)
plt.plot(df.CocoaPercentage, line_function(df.CocoaPercentage), "r--")
plt.show()
plt.clf()

#Where are the best cocoa beans grown?
best_cocoa = [country.get_text() for country in soup.select(".BroadBeanOrigin")[1:]]

df["BroadBeanOrigin"] = best_cocoa
best_beans_origin = df.groupby("BroadBeanOrigin").mean().Rating.nlargest(10)
print(best_beans_origin)

#Which countries produce the highest-rated bars?
best_producer = [producer.get_text() for producer in soup.select(".CompanyLocation")[1:]]
df["CompanyLocation"] = best_producer
best_country_producer = df.groupby("CompanyLocation").mean().Rating.nlargest(10)
print(best_country_producer)


