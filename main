import requests
import csv

def fetch_recent_books(limit=50):
    """
    Fetches books from OpenLibrary API and filters those published after 2000.
    Returns a list of dictionaries containing book details.
    """
    base_url = "https://openlibrary.org/search.json"
    params = {
        'q': 'programming',  
        'limit': limit
    }
    
    filtered_books = []
    
    try:
        response = requests.get(base_url, params=params)
        response.raise_for_status()
        data = response.json()
        
        for doc in data.get('docs', []):
            publish_year = doc.get('first_publish_year')
            
            if publish_year and publish_year > 2000:
                filtered_books.append({
                    'title': doc.get('title'),
                    'author': doc.get('author_name', ['Unknown'])[0],
                    'publish_year': publish_year
                })
                
        return filtered_books

    except requests.exceptions.RequestException as e:
        print(f"Error fetching data: {e}")
        return []

def save_to_csv(books, filename="recent_books.csv"):
    """Saves the list of books to a CSV file."""
    if not books:
        print("No books found to save.")
        return

    keys = books[0].keys()
    try:
        with open(filename, 'w', newline='', encoding='utf-8') as output_file:
            dict_writer = csv.DictWriter(output_file, fieldnames=keys)
            dict_writer.writeheader()
            dict_writer.writerows(books)
        print(f"Successfully saved {len(books)} books to {filename}")
    except IOError as e:
        print(f"Error writing to file: {e}")

if __name__ == "__main__":
    book_data = fetch_recent_books(50)
    save_to_csv(book_data)
    
