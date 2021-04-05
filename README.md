# Election Analysis using Python

## Overview 

The goal of the project was to complete an election audit of a local congressional election using python programming language.

### Tasks

#### How many votes were cast in this congressional election?
369,711
#### Provide a breakdown of the number of votes and the percentage of total votes for each county in the precinct.
County Votes:
Jefferson: 10.5% (38,855)
Denver: 82.8% (306,055)
Arapahoe: 6.7% (24,801)
#### Which county had the largest number of votes?
Denver
#### Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.
Charles Casper Stockham: 23.0% (85,213)
Diana DeGette: 73.8% (272,892)
Raymon Anthony Doane: 3.1% (11,606)
#### Which candidate won the election, what was their vote count, and what was their percentage of the total votes?
Winner: Diana DeGette
Winning Vote Count: 272,892
Winning Percentage: 73.8%

## Code

#Add our dependencies.
import csv
import os

#Add a variable to load a file from a path.
file_to_load = os.path.join("election_results.csv")
#Add a variable to save the file to a path.
file_to_save = os.path.join( "election_results.txt")

#Initialize a total vote counter.
total_votes = 0

#Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

#1: Create a county list and county votes dictionary.

county_op= []
countyvotes={}

#Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percent = 0

#2: Track the largest county and county voter turnout.
largest_countyturnout = ""
largest_number_turnout = 0


#Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

         # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_op:
            

            # 4b: Add the existing county to the list of counties.
            county_op.append(county_name)

            # 4c: Begin tracking the county's vote count.
            countyvotes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        countyvotes[county_name] += 1


#Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

        # 6a: Write a for loop to get the county from the county dictionary.
    for county_name in countyvotes:
        # 6b: Retrieve the county vote count.
        vote_county = countyvotes[county_name]
        # 6c: Calculate the percent of total votes for the county.
        vote_county_percentage = float(vote_county) / float(total_votes) * 100

         # 6d: Print the county results to the terminal.
        county_results = (f"{county_name}: {vote_county_percentage:.1f}% ({vote_county:,})\n")
        print(county_results)
         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (vote_county > winning_count):
           
            winning_count = vote_county
            largest_county_turnout = county_name
    # 7: Print the county with the largest turnout to the terminal.
    largest_county_results = (
        f"\n"
        f"----------------------------------------\n"
        f"Largest County Turnout: {largest_county_turnout}\n"
        f"----------------------------------------\n")
    print(largest_county_results)
    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(largest_county_results)
    
    # Save the final candidate vote count to the text file.
    winning_count = 0
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percent):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percent = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percent:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)

## Election audit results



## Summary 
The script can be modified to get the most quantity of votes from different cities assuming that the data includes this kind of information. Breaking down the analysis this way we could go deeper in the analysis. 

The code can also be changed to determine patterns testing the percentage of votes by city against each candidate. This would tell us which candidate was the most popular in each city or county.

