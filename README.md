import datetime as dt
import requests

BASE_URL = "http://api.openweathermap.org/data/2.5/weather?"
API_KEY = "e2643233c05c6585ba6cdd67ff658531"
city = "Portland"


def kelvin_to_celsius_fahrenheit(kelvin):
    celcius = kelvin - 273.15
    fahrenheit = celcius * 1.8 + 32 # (9/5) +32
    return fahrenheit, celcius

url = BASE_URL + "appid=" + API_KEY + "&q=" + city

response = requests.get(url).json()

temp_kelvin = response["main"]["temp"]
temp_celcius, temp_farhennhit = kelvin_to_celsius_fahrenheit(temp_kelvin)
feels_like_kelvin = response["main"]["feels_like"]
feels_like_celsius, feels_like_farhennhit = kelvin_to_celsius_fahrenheit(feels_like_kelvin)
wind_speed = response["wind"]["speed"]
humidity = response["main"]["humidity"]
description = response["weather"][0]["description"]
sunset_time = dt.datetime.fromtimestamp(response["sys"]["sunset"] + response["timezone"], dt.UTC)
sunrise_time = dt.datetime.fromtimestamp(response["sys"]["sunrise"] + response["timezone"], dt.UTC)
print(f"Tempature in {city}: {temp_celcius:.2f}F or {temp_farhennhit:.2f}C")
print(f"Tempature in {city} feels like: {feels_like_celsius:.2f}F or {feels_like_farhennhit:.2f}C")
print(f"Humidity: {humidity}%")
print(f"Wind speed: {wind_speed}m/s")
print(f"General weather in {city}: {description}")
print(f"Sun rises in {city} at {sunrise_time} local time.")
print(f"Sun sets in {city} at {sunset_time} local time.")
