# Food With Friends 

## Introduction

### The Problem

I have had trouble deciding where to eat with friends, so I decided to build a simple web application. 

The purpose is to build a log in and follower system, then invite people into groups. These groups will allow the user and their friends to find restaurants around their area using a nice Tinder-like system.

### Flow Of Events

- The user logs In
- The user can post a request to go out with friends
- When their friends agree, they input the type of food they are hungry for.
- They swipe left/right (dislike/like) on the topic of foods they want, and that curates 3 topics, and compiles a list of restaurants from those topics.
- They then get another list of actual restaurants from those topics, and the group votes on them using the same swipe system (left/right)
- After everyone goes through the list, the restaurant with the best score will be the winner. 
- People can vote again, or throw another category into the ring.

## Ranking

The ranking system for the restaurants will be calculated as followed:

- P: The number of points that a restaurant gets (swiped right, meaning V - P = the number of people who swiped left or had no preference)
- R: The number of restaurants that are in the list
- V: The number of people in the group
- Us: Total amount of people who wanted that type of food
- Ay: Average Yelp Rating for that restaurant
- **Formula:** Us + ( (P * V * Ay)  / (V-P) )

**Calculating the ranking runtime: O(1)**

**Sorting and ranking the restaurants runtime: O(n) (using max(), we will take the restaurant with the highest score.)**

The rank of a category will be determined linearly. It will just count the votes (right swipes) that are present.

The ranks of individual restaurants and categories will be calculated during the 

## Progress

Currently, I am rescaling what I already have into a cleaner format in Flask, and I am working on implementing the pages.

### API Integration

The posts and users are related to each other in a one-to-many relationship, however, the type of restaurant will be pulled from Yelp's API using the requests module. 

The categories of foods will be a dictionary, with a Boolean value to dictate if the current_user likes or dislikes that category, then that gets searched in Yelp's API for the type of restaurants, searching by 'term' in the API. The restaurants and category won't be stored in a table, and instead, it will be stored in a dictionary including the rank of said restaurants. 

To get information about the location of each person, we want to use Google's geolocation API using getCurrentPosition(). We want that to be our location, and we can set a radius of how far the list of restaurants are from said location.

We can then define the parameters of the API request call.

Code layout:

```python
for category in list_of_categories:
	# category => the category(ies) that the group has voted on 
	# NOTE: Will only work if the user allows location
	PARAMETERS = [
		'term': category,
	    'limit': 10,
    	'radius': 100,
    	'latitude': location.coords.latitude,
    	'longitude': location.coords.longitude
	]
    # PARAMETERS will be passed in request using the url location
```

We can pass in `PARAMETERS` into the request call, and compile the list of restaurants with the individual categories in a list called `restaurants`. 

### Layout

The web application is utilizing flask-bootstrap and the restaurant comparisons will be created myself. 

It won't have detailed information, but the cards will have the rating of the restaurant, the name, and the location of where it is, as well as a picture. 

When clicked on, it redirect to a page with more information pulled from a different url of the Yelp API, however, integration of this will come later.

### Database Layout

Currently, we have 3 tables: User and Posts and followers (reference table) in `models.py` and I will be adding another reference table to determine the groups of individuals (just extend followers to 4 people). Other than that, there won't be any other added elements to my database. Like I mentioned above, the restaurants won't actually be stored in our database, but instead, be private variables in a class, which will be calculating all of the changes there.

I also need to add a verification layer to the User class, to send a request to the other follower ID to accept current_user's follow request.

The summary code should look as so:

```python
...
# This will only be called with the person who you followed accepts your follow request
def verify(self, other_user):
    if not other_user.is_following(self):
        # adds follow
        self.followed.append(user)
    else:
        return f"{other_user} is already following you"
```

Pretty straight forward.

### TODO

- Create a "Add to group" page that also updates for other people
- Design a better landing page for index
- Design swipe mechanism

## Other Things to Note

The data in my current database is test data and not actual user data.



### Install Dependencies

```bash
pip3 install -r requirements.txt
```

