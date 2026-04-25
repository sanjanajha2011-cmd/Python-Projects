# Python-Projects
# database
books = []
Issued_books = {}

import datetime

# add function
def add_book():
    name = input('Enter the book name: ')
    books.append(name)
    print(name, 'is added successfully')


def show_books():
    if len(books) == 0:
        print('No books available')
    else:
        print('Available books:')
        for b in books:
            print(b)


def issue_book():
    if len(books) == 0:
        print('No books available')
        return
    else:
        show_books()
        name = input('Enter the book name you want to issue: ')
        
        if name in books:
            student = input('Enter student name: ')
            days = int(input('Enter duration (in days): '))
            
            issue_date = datetime.date.today()
            
            Issued_books[name] = {
                "student": student,
                "issue_date": issue_date,
                "duration": days
            }
            
            books.remove(name)
            print(name, 'is issued to', student)
        else:
            print(name, 'is not available')


def calculate_fine(issue_date, duration):
    today = datetime.date.today()
    return_date = issue_date + datetime.timedelta(days=duration)
    
    if today <= return_date:
        return 0
    
    late_days = (today - return_date).days
    weeks = late_days // 7 + 1
    
    fine = 0
    for i in range(1, weeks + 1):
        fine += i * 10   # progressive fine
    
    return fine


def return_book():
    name = input('Enter the book name you want to return: ')
    
    if name in Issued_books:
        data = Issued_books[name]
        
        fine = calculate_fine(data["issue_date"], data["duration"])
        
        if fine > 0:
            print('Late return! Fine = ₹', fine)
        else:
            print('Returned on time. No fine.')
        
        books.append(name)
        del Issued_books[name]
        
        print(name, 'book returned')
    else:
        print(name, 'book was never issued')


def library():
    while True:
        print('\n1. Add Book')
        print('2. Show Books')
        print('3. Issue Book')
        print('4. Return Book')
        print('5. Exit')
        
        choice = int(input('Enter your choice: '))
        
        if choice == 1:
            add_book()
        elif choice == 2:
            show_books()
        elif choice == 3:
            issue_book()
        elif choice == 4:
            return_book()
        elif choice == 5:
            print('Thank You')
            break
        else:
            print('Invalid choice')


# run program
library()