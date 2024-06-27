<!-- This is the markdown template for the final project of the Building AI course, 
created by Reaktor Innovations and University of Helsinki. 
Copy the template, paste it to your GitHub README and edit! -->

## Spliting Fare
Split the bill for the cab with friends

## Summary
When at least two friends want to take Uber/Yango/Bolt to return each home, this AI assist to calculate the bill and split the amount based on the distance for each. For example, if two people leave from HEL airport to each home, one in Töölö and one in Tapiola, the taxi stops first in Tapiola via Kehä 1. After then, it goes to the second destination, Töölö. Based on the total amount of the cost, two bills should be prepared. For the first passenger, it can be 1/2 of the distance from the airport and 1/2 of the distance which the second passenger had to detour to drop by the first destination. The second passenger will pay the rest of it.


## Background
During summer time, there are many travels and parties all over the Uusima area. It's always nice to walk, cycle, or take the public transportation. However, there are moments when we are tired or too drunken. In this case, taxi is the best option, espcially if we have friedns to join the journey. However, it's a bit difficult to calculate the exact amount to split as the destinations are different. I was thinking if the fee can be splited and calculated automatically, we don't have to spend time for the calculating and share the bills, esepcially when we are so tired.


## How is it used?
As mentioned previously, it can be used when we want to split the bills for the cap with friends. So the main users can be Uusima people who can afford and are willing to use cab. For doing this, it can be integrated into the current Taxi applications.

<Visual example for the UI>
  -------------------------------------------------
|               [App Title]                     |
|               [Subtitle]                      |
-------------------------------------------------
| Start Point Input | Destination Inputs        |
|                   | [Destination Field]        |
|                   | [Destination Field]        |
|                   | [Add Destination Button]   |
-------------------------------------------------
|                [Map Display]                  |
|          [Route Visualization]                |
-------------------------------------------------
| Total Fare Estimate | Individual Fare Breakdowns|
|                     | [Passenger 1 Fare]        |
|                     | [Passenger 2 Fare]        |
|                     | [Passenger 3 Fare]        |
|                     |          ...              |
|                     | [Calculate and Split Button]|
-------------------------------------------------
|                 [Result Display]               |
|               [Detailed Breakdown]             |
-------------------------------------------------

<Distance calculation code>
import requests

def get_distance_and_duration(origin, destination, api_key):
    url = f"https://maps.googleapis.com/maps/api/distancematrix/json?origins={origin}&destinations={destination}&key={api_key}"
    response = requests.get(url)
    result = response.json()
    distance = result["rows"][0]["elements"][0]["distance"]["value"] / 1000  # in km
    duration = result["rows"][0]["elements"][0]["duration"]["value"] / 60  # in minutes
    return distance, duration

# Example
origin = "Helsinki Airport"
destination = "Töölö, Helsinki"
api_key = "YOUR_GOOGLE_MAPS_API_KEY"
distance, duration = get_distance_and_duration(origin, destination, api_key)
print(f"Distance: {distance} km, Duration: {duration} minutes")

<Fare calculation code>
def calculate_fare(distance, base_fare=3.0, per_km_rate=1.5):
    return base_fare + (distance * per_km_rate)

# Example
distance = 15.0  # km
fare = calculate_fare(distance)
print(f"Fare: {fare} euros")

<Spliting fare code>
def split_fare(total_fare, distances):
    total_distance = sum(distances)
    shares = [distance / total_distance for distance in distances]
    individual_fares = [share * total_fare for share in shares]
    return individual_fares

# Example
distances = [10.0, 5.0]  # Distances for each passenger
total_fare = 30.0  # Total fare in euros
individual_fares = split_fare(total_fare, distances)
print(f"Individual Fares: {individual_fares}")


## Data sources and AI methods
Data can come from Google map, or the taxi application data base.

## Challenges
The biggest challenge is collobrating the current taxi company.

## What next?
This can be also used for the daily bill splits. For example, this can provide the bills during the common dinner or drinks such as wine bottles.

## Acknowledgments
My inspiration was based on my needs. I sometimes take the cap with friends and it's difficult to split the bills properly. Of course, as friends, I can pay more but it'd be great if we don't have to care about this process.
