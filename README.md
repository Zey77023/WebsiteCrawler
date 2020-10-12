# Web Crawler(Not Exactly!)
    As the header says, this is not exactly a web
    crawler, but a Web-Site crawler.
## Code
    This program was written in python and it is a
    fairly small piece of code.
```python
import requests
from urllib.parse import urlparse, urljoin
from bs4 import BeautifulSoup
import colorama
import sys


sys.setrecursionlimit(99999999)


print("WEBSITE CRAWLER".center(175,"_"))
print("\n","="*175)
print("\n\n\n\nThis program does not tolerate faults!\nPlease type whatever you are typing correctly!\nIf you think you have made a mistake please close the program and reopen it!\nIf you proceed with errors the program will crash/close!\nHelp can be found in the README.txt file!\n\n\n")
print("\n","="*175)


siteurl = input("Enter the address of the site (Please don't forget https:// or http://, etc. at the front!) :")
max_urls = int(input("Enter the number of urls you want to crawl through the main page : "))
filename = input("Give a name for your text file (Don't append .txt at the end!) : ")


# init the colorama module
colorama.init()
GREEN = colorama.Fore.GREEN
MAGENTA = colorama.Fore.MAGENTA
RESET = colorama.Fore.RESET


# initialize the set of links (unique links)
internal_urls = set()
external_urls = set()


def is_valid(url):
    """
    Checks whether `url` is a valid URL.
    """
    parsed = urlparse(url)
    return bool(parsed.netloc) and bool(parsed.scheme)


def get_all_website_links(url):
    """
    Returns all URLs that is found on `url` in which it belongs to the same website
    """
    # all URLs of `url`
    urls = set()
    # domain name of the URL without the protocol
    domain_name = urlparse(url).netloc
    soup = BeautifulSoup(requests.get(url).content, "html.parser")
    r = requests.get(url, allow_redirects=False)
    for a_tag in soup.findAll("a"):
    #check for connection errors
        try:
            href = a_tag.attrs.get("href")
        except ConnectionError or ConnectionResetError:
            if not href == href1:
                with open(filename+".txt","a") as f:
                    try:
                        print(f"{href}",file = f)
                    except UnicodeEncodeError:
                        continue
            print(f"{RED} [#]Error Occured!")
            continue
        if href == "" or href is None:
            # href empty tag
            continue
        # join the URL if it's relative (not absolute link)
        href = urljoin(url, href)
        parsed_href = urlparse(href)
        # remove URL GET parameters, URL fragments, etc.
        href = parsed_href.scheme + "://" + parsed_href.netloc + parsed_href.path
        if not is_valid(href):
            # not a valid URL
            continue
        if href in internal_urls:
            # already in the set
            continue
        if domain_name not in href:
            # external link
            if href not in external_urls:
                print(f"{MAGENTA} [!] External link: {href}{RESET}")
                with open(filename+".txt","a") as f:
                    try:
                        print(f"{href}",file = f)                        
                    except UnicodeEncodeError:
                        continue
                external_urls.add(href)
            continue
        print(f"{GREEN} [*] Internal link: {href}{RESET}")
        with open(filename+".txt","a") as f:
            try:
                print(f"{href}",file = f)
            except UnicodeEncodeError:
                continue
        href1 = href
        urls.add(href)
        internal_urls.add(href)
    return urls



# number of urls visited so far will be stored here
total_urls_visited = 0

def crawl(url, max_urls=50000):
    """
    Crawls a web page and extracts all links.
    You'll find all links in `external_urls` and `internal_urls` global set variables.
    params:
        max_urls (int): number of max urls to crawl, default is 50000.
    """
    global total_urls_visited
    total_urls_visited += 1
    links = get_all_website_links(url)
    for link in links:
        if total_urls_visited > max_urls:
            break
        crawl(link, max_urls=max_urls)



if __name__ == "__main__":
    crawl(siteurl,max_urls)
    print("[+] Total External links:", len(external_urls))
    print("[+] Total Internal links:", len(internal_urls))
    print("[+] Total:", len(external_urls) + len(internal_urls))
    input("Press any key to exit...")
```    
## Build
    This program was built using pyinstaller, a python
    package which can be installed using
    ```
    pip install pyinstaller
    ```
## Info
This is my first time in GitHub and my first project too.

This might not work in the way you intend it to work so,
feel free to make changes to the code and personalize it
to your needs!

**This repo will not be further maintained!(Pardon me if my grammar is bad)**
