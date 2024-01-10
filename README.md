# patient-form
import json

# Function to load users' data from JSON file
def load_users():
    try:
        with open('users.json', 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return {}

# Function to save users' data to JSON file
def save_users(users):
    with open('users.json', 'w') as file:
        json.dump(users, file, indent=4)

# Function for user login
def login():
    users = load_users()
    username = input("Enter username: ")
    password = input("Enter password: ")
    if username in users and users[username]['password'] == password:
        print("Login successful!")
        return True, username
    else:
        print("Invalid username or password.")
        return False, None

# Function for patient registration
def register(users):
    print("\nPatient Registration")
    name = input("Enter name: ")
    age = input("Enter age: ")
    gender = input("Enter gender: ")
    contact = input("Enter contact number: ")

    user_data = {
        'name': name,
        'age': age,
        'gender': gender,
        'contact': contact,
        'status': 'Pending'
    }
    
    users[name] = user_data
    save_users(users)
    print(f"\n{ name }'s information has been registered.")

# Function to display all registered patients' data
def display_patients():
    users = load_users()
    print("\nRegistered Patients:")
    for username, data in users.items():
        print(f"Name: { data['name'] }, Age: { data['age'] }, Gender: { data['gender'] }, Contact: { data['contact'] }, Status: { data['status'] }")

# Main program
def main():
    while True:
        print("\n--- Patient Management System ---")
        print("1. Login")
        print("2. Register")
        print("3. Display Patients")
        print("4. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            logged_in, username = login()
            if logged_in:
                while True:
                    print("\n--- Welcome! ---")
                    print("1. Register a Patient")
                    print("2. Display Registered Patients")
                    print("3. Logout")
                    option = input("Enter your option: ")
                    
                    if option == '1':
                        register(load_users())
                    elif option == '2':
                        display_patients()
                    elif option == '3':
                        break
                    else:
                        print("Invalid option.")
        elif choice == '2':
            register(load_users())
        elif choice == '3':
            display_patients()
        elif choice == '4':
            print("Exiting program...")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
