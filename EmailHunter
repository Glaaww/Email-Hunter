import requests
import time
from colorama import Fore, Style, init


init(autoreset=True)

def validate_email(email):
    """Validate the format of the email address."""
    return "@" in email and "." in email.split("@")[-1]

def validate_input(name, location, affiliation):
    """Validate user input."""
    if not name or not location:
        raise ValueError("Name and location cannot be empty.")
    if len(name.split()) < 2:
        raise ValueError("Please enter the full name (first and last).")
    if not affiliation:
        raise ValueError("Affiliation is required for better results.")

def get_email_candidates(name, location, affiliation):
    """Retrieve email candidates from public data sources."""
    
    api_key = 'Api key dal idher loru'
    email_candidates = []
    
    sources = [
        f"https://api.hunter.io/v2/domain-search?domain={affiliation}&api_key={api_key}",
        f"https://api.clearbit.com/v2/people/find?name={name}&domain={affiliation}&api_key={api_key}"
    ]

    for source in sources:
        try:
            response = requests.get(source)
            response.raise_for_status()  
            data = response.json()
            emails = data.get('emails', [])
            for email_info in emails:
                email = email_info.get('value')
                if validate_email(email):
                    email_candidates.append({
                        'email': email,
                        'confidence': email_info.get('confidence', 0)
                    })
        except requests.exceptions.RequestException as e:
            print(f"{Fore.RED}Error fetching data from {source}: {e}")
        time.sleep(1) 

    return email_candidates

def display_results(email_candidates):
    """Display the found email candidates."""
    if not email_candidates:
        print(Fore.YELLOW + "No email addresses found.")
        return

    print(Fore.GREEN + "\nFound Email Addresses:")
    for candidate in email_candidates:
        print(Fore.CYAN + f"Email: {candidate['email']}, Confidence: {candidate['confidence']}")

def log_search(name, location, affiliation, results):
    """Log search attempts for compliance."""
    with open('search_log.txt', 'a') as log_file:
        log_file.write(f"Search: Name: {name}, Location: {location}, Affiliation: {affiliation}, Results: {results}\n")

def display_credits():
    """Display credits and license information."""
    print(Fore.MAGENTA + "\nCredits:")
    print(Fore.MAGENTA + "Developed by Eren AKA Glaaww")
    print(Fore.MAGENTA + "License: GNU General Public License v3.0")
    print(Fore.RED + "If you reuse this code without permission, actions will be taken against it.\n")

def main():
    """Main function to execute the email locator tool."""
    print(Fore.BLUE + "Welcome to the Advanced Email Locator Tool!")

    try:
        
        name = input(Fore.WHITE + "Enter the person's full name: ")
        location = input(Fore.WHITE + "Enter the location (city, state): ")
        affiliation = input(Fore.WHITE + "Enter known affiliations (company/school): ")

        
        validate_input(name, location, affiliation)

        
        email_candidates = get_email_candidates(name, location, affiliation)

        
        log_search(name, location, affiliation, email_candidates)

        
        display_results(email_candidates)

    except ValueError as ve:
        print(f"{Fore.RED}Input Error: {ve}")
    except Exception as e:
        print(f"{Fore.RED}An unexpected error occurred: {e}")

    
    display_credits()

if __name__ == "__main__":
    main()
