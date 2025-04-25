here Our task is :
(1)Handle missing values
(2)Remove duplicate or inconsistent data
(3)Standardize the data format

Task 1: Identify Issues in the Data
Your manager provides you with an example dataset where some records are incomplete or incorrect. Here’s an example:

{
    "users": [
        {"id": 1, "name": "Amit", "friends": [2, 3], "liked_pages": [101]},
        {"id": 2, "name": "Priya", "friends": [1, 4], "liked_pages": [102]},
        {"id": 3, "name": "", "friends": [1], "liked_pages": [101, 103]},
        {"id": 4, "name": "Sara", "friends": [2, 2], "liked_pages": [104]},
        {"id": 5, "name": "Amit", "friends": [], "liked_pages": []}
    ],
    "pages": [
        {"id": 101, "name": "Python Developers"},
        {"id": 102, "name": "Data Science Enthusiasts"},
        {"id": 103, "name": "AI & ML Community"},
        {"id": 104, "name": "Web Dev Hub"},
        {"id": 104, "name": "Web Development"}
    ]
}


Problems:
User ID 3 has an empty name.
User ID 4 has a duplicate friend entry.
User ID 5 has no connections or liked pages (inactive user).
The pages list contains duplicate page IDs.
Task 2: Clean the Data
We will:

Remove users with missing names.
Remove duplicate friend entries.
Remove inactive users (users with no friends and no liked pages).
Deduplicate pages based on IDs.
Code Implementation
import json
 
def clean_data(data):
    # Remove users with missing names
    data["users"] = [user for user in data["users"] if user["name"].strip()]
    
    # Remove duplicate friends
    for user in data["users"]:
        user["friends"] = list(set(user["friends"]))
    
    # Remove inactive users
    data["users"] = [user for user in data["users"] if user["friends"] or user["liked_pages"]]
    
    # Remove duplicate pages
    unique_pages = {}
    for page in data["pages"]:
        unique_pages[page["id"]] = page
    data["pages"] = list(unique_pages.values())
    
    return data
 
# Load, clean, and display the cleaned data
data = json.load(open("codebook_data.json"))
data = clean_data(data)
json.dump(data, open("cleaned_codebook_data.json", "w"), indent=4)
print("Data cleaned successfully!")

Expected Output:
{
    "users": [
        {"id": 1, "name": "Amit", "friends": [2, 3], "liked_pages": [101]},
        {"id": 2, "name": "Priya", "friends": [1, 4], "liked_pages": [102]},
        {"id": 4, "name": "Sara", "friends": [2], "liked_pages": [104]}
    ],
    "pages": [
        {"id": 101, "name": "Python Developers"},
        {"id": 102, "name": "Data Science Enthusiasts"},
        {"id": 103, "name": "AI & ML Community"},
        {"id": 104, "name": "Web Development"}
    ]
}

The cleaned dataset will:

Remove users with missing names
Ensure friend lists contain unique entries
Remove inactive users
Deduplicate pages
