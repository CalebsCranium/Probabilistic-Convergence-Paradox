#Probabilistic Convergence of Birthdays: Navigating the Paradox

# Import necessary modules
import datetime  # Module to work with dates and times
import random    # Module to generate random numbers

# Function to generate random birthdays
def getBirthdays(numberOfBirthdays):
    # Create an empty list to store the generated birthdays
    birthdays = []
    for i in range(numberOfBirthdays):
        # Set the start of the year for the simulation
        startOfYear = datetime.date(2001, 1, 1)
        # Generate a random number of days within a year
        randomNumberofDays = datetime.timedelta(days=random.randint(0, 364))
        # Calculate the birthday by adding the random days to the start of the year
        birthday = startOfYear + randomNumberofDays
        # Add the generated birthday to the list
        birthdays.append(birthday)
    # Return the list of generated birthdays
    return birthdays

# Function to find a matching birthday
def getMatch(birthdays):
    """Returns the date object of a birthday that occurs more than once
    in the birthdays list."""
    if len(birthdays) == len(set(birthdays)):
        return None  # All birthdays are unique, so return None.
    
    # Compare each birthday to every other birthday:
    for a, birthdayA in enumerate(birthdays):
        for birthdayB in birthdays[a+1:]:
            if birthdayA == birthdayB:
                return birthdayA  # Return the matching birthday.

# Display the introduction
print('''Probabilistic Convergence of Birthdays: Navigating the Paradox,
      by Caleb Cooper

      This probabilistic convergence of birthdays shows us that in a group of N people,
      the odds that two of them have matching birthdays is shockingly large.
      This program performs a Monte Carlo simulation (repeated random
      simulations) to explore this concept.
      
      (It is not actually a paradox, it is just a surprising result.)
      ''')

# Set up a tuple of month names in order:
MONTHS = ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
          'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec')

while True:
    # Keep asking until the user enters a valid amount.
    print('How many birthdays shall I generate? (Max 100)')
    response = input('> ')
    if response.isdecimal() and (0 < int(response) <= 100):
        numBDays = int(response)
        break  # User has entered a valid amount

# Generate and display the birthdays:
print('Here are', numBDays, 'birthdays:')
birthdays = getBirthdays(numBDays)
for i, birthday in enumerate(birthdays):
    if i != 0:
        print(',', end=' ')  # Print comma separator between birthdays
    monthName = MONTHS[birthday.month - 1]  # Get the month name based on the month number
    dateText = '{} {}'.format(monthName, birthday.day)  # Format the date text
    print(dateText, end=' ')  # Print the formatted date text
print()  # Print an empty line for separation
print()  # Print another empty line for separation

# Determine if there are two matching birthdays
match = getMatch(birthdays)

# Display the results:
print('In this simulation,', end=' ')
if match is not None:
    monthName = MONTHS[match.month - 1]
    dateText = '{} {}'.format(monthName, match.day)
    print('multiple people have a birthday on', dateText)
else:
    print('there are no matching birthdays.')
print()  # Print an empty line for separation

# Run through 100,000 simulations
print('Generating', numBDays, 'random birthdays 100,000 times...')
input('Press Enter to begin...')

print('Let\'s run another 100,000 simulations.')
simMatch = 0  # Counter for how many simulations had matching birthdays
for i in range(100_000):
    if i % 10_000 == 0:
        print(i, 'simulations run...')  # Print progress every 10,000 simulations
    birthdays = getBirthdays(numBDays)
    if getMatch(birthdays) is not None:
        simMatch += 1  # Increment the counter if a match is found
print('100,000 simulations run.')

# Display simulation results:
probability = round(simMatch / 100_000 * 100, 2)  # Calculate the probability of matching birthdays
print('Out of 100,000 simulations of', numBDays, 'people, there was a')
print('matching birthday in that group', simMatch, 'times. This means')
print('that', numBDays, 'people have a', probability, '% chance of')
print('having a matching birthday in their group.')
print('That\'s probably more than you would think!')
