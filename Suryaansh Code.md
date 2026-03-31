import csv

from datetime import datetime

EXPENSE\_FILE = 'expenses.csv'

def initialize\_file():

"""Initializes the CSV file with headers if it doesn't exist."""

try:

with open(EXPENSE\_FILE, 'x', newline='') as file:

writer = csv.writer(file)

writer.writerow(\['Date', 'Category', 'Amount', 'Description'\])

except FileExistsError:

pass # File already exists

def add\_expense(date, category, amount, description):

"""Adds a new expense to the CSV file."""

with open(EXPENSE\_FILE, 'a', newline='') as file:

writer = csv.writer(file)

writer.writerow(\[date, category, amount, description\])

print("Expense added successfully.")

def view\_expenses():

"""Displays all recorded expenses."""

initialize\_file() # Ensure file exists before reading

with open(EXPENSE\_FILE, 'r') as file:

reader = csv.reader(file)

header = next(reader) # Skip header

print(f"\\n{' | '.join(header)}")

print("-" \* (len(' | '.join(header)) + 2))

for row in reader:

print(f"{' | '.join(row)}")

def get\_monthly\_summary(month, year):

"""Calculates and displays the total expenses for a given month and year."""

total\_expenses = 0

category\_summary = {}

initialize\_file()

with open(EXPENSE\_FILE, 'r') as file:

reader = csv.reader(file)

next(reader) # Skip header

for row in reader:

expense\_date\_str = row\[0\]

expense\_date = datetime.strptime(expense\_date\_str, '%Y-%m-%d')

if expense\_date.month == month and expense\_date.year == year:

try:

amount = float(row\[2\])

category = row\[1\]

total\_expenses += amount

category\_summary\[category\] = category\_summary.get(category, 0) + amount

except ValueError:

print(f"Skipping invalid amount: {row\[2\]}")

print(f"\\n--- Monthly Summary for {datetime(year, month, 1).strftime('%B %Y')} ---")

print(f"Total Expenses: ${total\_expenses:.2f}")

print("Expenses by Category:")

for category, amount in category\_summary.items():

print(f" {category}: ${amount:.2f}")

print("-" \* 40)

def main():

initialize\_file()

while True:

print("\\nMonthly Expenditure Tracker")

print("1. Add Expense")

print("2. View Expenses")

print("3. Monthly Summary")

print("4. Exit")

choice = input("Enter your choice: ")

if choice == '1':

date = input("Enter date (YYYY-MM-DD): ")

category = input("Enter category: ")

amount = input("Enter amount: ")

description = input("Enter description: ")

add\_expense(date, category, amount, description)

elif choice == '2':

view\_expenses()

elif choice == '3':

try:

month = int(input("Enter month (1-12): "))

year = int(input("Enter year (YYYY): "))

get\_monthly\_summary(month, year)

except ValueError:

print("Invalid month or year. Please enter numbers.")

elif choice == '4':

print("Exiting tracker. Goodbye!")

break

else:

print("Invalid choice. Please try again.")

if \_name\_ == "\_main\_":

main()