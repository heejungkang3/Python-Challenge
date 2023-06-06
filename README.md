# Python-Challenge
# Modules
import os
import csv

# Set path for file
csvpath = os.path.join("..","Python-Challenge","budget_data.csv")


# Set variables as initial of zero
month_count = 0
total_profit_losses = 0
prior_profit = 0

# Had to make list to get the dates and max/min
monthly_change_list = []
date_list = []

# Open the CSV file
with open(csvpath) as csvfile:
    csvreader = csv.reader(csvfile, delimiter=',')
    
    csv_header = next(csvreader)
    date = csv_header.index("Date")
    total_profit_losses = csv_header.index("Profit/Losses")

    for row in csvreader:
        # The total number of months included in the dataset
        month_count = month_count + 1

        # The net total amount of "Profit/Losses" over the entire period
        total_profit_losses = total_profit_losses + int(row[1])
      
        # The changes in "Profit/Losses" over the entire period, and then the average of those changes
        
        current_profit = int(row[1])
        monthly_change = current_profit- prior_profit
        monthly_change_list.append(monthly_change)
        prior_profit = int(row[1])

        date_list.append(row[0])

# Get rid of the first change as it does not have a prior 
monthly_change_list.pop(0)

# Average of profits
avg_change = sum(monthly_change_list)/len(monthly_change_list)
avg_change = round(avg_change,2)

# The greatest increase in profits (date and amount) over the entire period
greatest_change = max(monthly_change_list)
greatest_change_position = monthly_change_list.index(greatest_change) + 1
greatest_change_date = date_list[greatest_change_position]

# The greatest decrease in profits (date and amount) over the entire period
smallest_change = min(monthly_change_list)
smallest_change_position = monthly_change_list.index(smallest_change) + 1
smallest_change_date = date_list[smallest_change_position]

print("Financial Analysis")
print("----------------------------------")
print("Total Months: " + str(month_count))
print("Total: " + str(total_profit_losses))
print("Average Change: $" + str(avg_change))
print("Greatest Increase in Profits: " + str(greatest_change_date) + " ($" + str(greatest_change) + ")")
print("Greatest Decrease in Profits: " + str(smallest_change_date) + " ($" + str(smallest_change) + ")")

#-------------------------------------------------------------------------------------------------------------------------
csvpath1 = os.path.join("..", "Python-Challenge", "election_data.csv")

vote_count = 0
prev_candidate = ""
candidate_list = []
full_candidate_list = []

with open(csvpath1) as csvfile1:
    csvreader1 = csv.reader(csvfile1, delimiter=',')

    csv_header1 = next(csvreader1)
    ballot_id = csv_header1.index("Ballot ID")
    county = csv_header1.index("County")
    candidate = csv_header1.index("Candidate")
    stockham_count = 0
    degette_count = 0
    doane_count = 0

    for row in csvreader1:

        # The total number of votes cast
        vote_count = vote_count + 1 

        full_candidate_list.append(row[2])

        # A complete list of candidates who received votes
        row_candidate = row[candidate]
        if row_candidate != prev_candidate:
            candidate_list.append(row_candidate)
        prev_candidate = row_candidate


        if row[2] == candidate_list[0]:
            stockham_count = stockham_count + 1
        elif row[2] == candidate_list[1]:
            degette_count = degette_count + 1
        elif row[2] == candidate_list[2]:
            doane_count = doane_count + 1

    # Get rid of repeat candidates in csv file       
    candidate_list.pop(3)
    candidate_list.pop(3)
    candidate_list.pop(3)
    candidate_list.pop(3)
    candidate_list.pop(3)
    candidate_list.pop(3)

    # The percentage of votes each candidate won
    stockham_percentage = round(((stockham_count/vote_count)*100),2)
    degette_percentage = round(((degette_count/vote_count)*100),2)
    doane_percentage = round(((doane_count/vote_count)*100),2)

    # The winner of the election
    percentage_list = [stockham_percentage,degette_percentage,doane_percentage]
    winner_perc = max(percentage_list)
    winner_perc_index = percentage_list.index(winner_perc)
    winner = candidate_list[winner_perc_index]


    

      
print("    ")        
print("Election Results")
print("----------------------------------")
print("Total Votes: " + str(vote_count))
print("----------------------------------")
print(str(candidate_list[0]) + ": " + str(stockham_percentage) + "% (" + str(stockham_count) + ")")
print(str(candidate_list[1]) + ": " + str(degette_percentage) + "% (" + str(degette_count) + ")")
print(str(candidate_list[2]) + ": " + str(doane_percentage) + "% (" + str(doane_count) + ")")
print("----------------------------------")
print("Winner: " + str(winner))



