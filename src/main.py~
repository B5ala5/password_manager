import csv
import os
import pyperclip

from cryptography.fernet import Fernet

from subprocess import call

from time import sleep


def write_key():
    # Generate key and save it

    key = Fernet.generate_key()

    with open("key.key", "wb") as key_file:
        key_file.write(key)

    print('Key generated')


def load_key():
    # Load the generated key in the current directory

    print('Key loaded')

    return open("key.key", "rb").read()


def encrypt(filename, key):
    # Takes filename(str) and key(bytes) to encrypt the file

    f = Fernet(key)

    with open(filename, "rb") as fn:
        data = fn.read()

    # Encrypt data
    encrypted_data = f.encrypt(data)

    # Write encrypted file
    with open(filename, "wb") as fn:
        fn.write(encrypted_data)

    print(f'File "{filename}" encrypted')


def decrypt(filename, key):
    # Takes filename(str) and key(bytes) to decrypt the file

    f = Fernet(key)

    with open(filename, "rb") as fn:
        encrypted_data = fn.read()

    # Decrypt data
    decrypted_data = f.decrypt(encrypted_data)

    # Write original file
    with open(filename, "wb") as fn:
        fn.write(decrypted_data)

    print(f'File "{filename}" decrypted')
   

def write_data(filename, data, key):

    fieldnames = ["App/Site", "Username", "Email", "Password"]

    f = Fernet(key)

    data['Password'] = f.encrypt(data['Password'].encode())

    data['Password'] = data['Password'].decode()


    if os.path.exists(filename):

        decrypt(filename, key)

        with open(filename, "r") as r_file:

            with open(filename, "a") as pass_file:

                pass_reader = csv.DictReader(r_file)

                exists = False

                for row in pass_reader:

                    if data['App/Site'] == row.get('App/Site'):
                        if data['Email'] == row.get('Email'):
                            exists = True
                        
                if exists:

                    print('That Email is already registered on that site')

                else:

                    pass_writer = csv.DictWriter(pass_file, fieldnames=fieldnames)
                    pass_writer.writerow(data)

                    print(f'File "{filename}" updated')


    else:

        with open(filename, "w") as pass_file:

            pass_writer = csv.DictWriter(pass_file, fieldnames=fieldnames)

            pass_writer.writeheader()
            pass_writer.writerow(data)

        print(f'File "{filename}" created')

    encrypt(filename, key)


def list_app_site(filename, key):

    app_site_list = []

    if os.path.exists(filename):

        decrypt(filename, key)

        with open(filename, "r") as pass_file:

            pass_reader = csv.DictReader(pass_file)

            for row in pass_reader:
                #for value in row.get('App/Site'):
                if row.get('App/Site') not in app_site_list:
                    app_site_list.append(row.get('App/Site'))
        
        encrypt(filename, key)

        print('\n')
        
        for i in range(len(app_site_list)):
            print(f'{i+1}. {app_site_list[i]}')
        
        print('\nPress Enter to return to menu')

    else:

        print('There is no App/Site stored')


def get_credentials(filename, app_site, key):

    credentials = {
            "App/Site": app_site,
            "Username": [],
            "Email": [],
            "Password": []
            }

    if os.path.exists(filename):

        decrypt(filename, key)

        with open(filename, "r") as pass_file:

            pass_reader = csv.DictReader(pass_file)

            for row in pass_reader:

                if row.get('App/Site').lower() == app_site.lower():

                    if row.get('Email') not in credentials['Email']:

                        credentials['Username'].append(row.get('Username'))
                        credentials['Email'].append(row.get('Email')) 
                        credentials['Password'].append(row.get('Password').encode()) 
        
        encrypt(filename, key)

        print('\n')

        f = Fernet(key)

        for i in range(len(credentials['Email'])):
            print(f'.::[{i+1}]::.')

            print(f'Username: {credentials["Username"][i]}')
            print(f'Email: {credentials["Email"][i]}')
         
            decrypted_password = f.decrypt(credentials['Password'][i])
            decrypted_password = decrypted_password.decode()
            credentials["Password"][i] = credentials["Password"][i].decode()

            print(f'Password: {decrypted_password}')
            print(f'Password encrypted: {credentials["Password"][i]}')
        
            print('\n')

        element = int(input('\nEnter the element number to copy the password: '))
        

        if element < 1 or element > len(credentials['Password']):
            
            print(f'\nPlease enter a number between 1 and {len(credentials["Password"])}')

        else:

            pyperclip.copy(credentials['Password'][element-1])

            print('\nPassword coppied to clipboard')

        print('\nPress Enter to return to menu')

    else:

        print('There is no credentials stored')


data = {
        "App/Site": "",
        "Username": "",
        "Email": "",
        "Password": ""
        }


def clear():

    _ = call('clear' if os.name == 'posix' else 'cls')

def menu():
    clear()

    print('.::Password Vault::.\n')
    print('1. Generate new key')
    print('2. Store a password')
    print('3. List App/Site stored')
    print('4. Find credentials from App/Site')
    print('5. Encrypt file "passwords.csv"')
    print('6. Decrypt file "passwords.csv"')
    print('7. Exit\n')

    option = int(input('[]> '))

    if option == 1:
        clear()
        
        print('.::1.Generate new key::.\n')

        write_key()
        
        sleep(2)
        clear()
        menu()

    if option == 2:
        clear()

        print('.::2. Store a Password::.\n')

        # Input Data:
        data['App/Site'] = str(input('App/Site: '))
        data['Username'] = str(input('Username: '))
        data['Email'] = str(input('Email: '))
        data['Password'] = str(input('Password: '))

        write_data('passwords.csv', data, key=load_key())
        
        input()
        clear()
        menu()

    if option == 3:
        clear()

        print('.::3. List App/Site stored::.\n')

        list_app_site('passwords.csv', key=load_key())

        input()
        clear()
        menu()

    if option == 4:
        clear()

        print('.::4. Find credentials from App/Site::.\n')

        app_site = str(input('App/Site: '))
        print('\n')

        get_credentials('passwords.csv', app_site, key=load_key())

        input()
        clear()
        menu()

    if option == 5:
        clear()

        encrypt('passwords.csv', key=load_key())

        sleep(2)
        clear()
        menu()

    if option == 6:
        clear()

        decrypt('passwords.csv', key=load_key())

        sleep(2)
        clear()
        menu()

    if option == 7:
        return 1

menu()
