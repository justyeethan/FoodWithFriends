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

The ranking system will be calculated as followed:

- P: The number of points that a restaurant gets (swiped right, meaning V - P = the number of people who swiped left or had no preference)
- R: The number of restaurants that are in the list
- V: The number of people in the group
- Us: Total amount of people who wanted that type of food
- Ay: Average Yelp Rating for that restaurant
- **Formula:** Us + ( (P * V * Ay)  / (V-P) )

## Progress

Currently, I am rescaling what I already have into a cleaner format in Flask, and I am working on implementing the pages.

### TODO

- Create a "Add to group" page that also updates for other people
- Design a better landing page for index
- Refine flow of events in blog postings and a delete request for posts and groups
- Allow users to get a way to accept or decline a follow
- Incorporate ranking algorithm into program
- Design swipe mechanism

## Other Things to Note

The data in my current database is test data and not actual user data.