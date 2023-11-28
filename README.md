# Google-Play-Store-code-Generator-
#By Vighnesh patil
#Buy me a coffee https://www.buymeacoffee.com/vighneshpatil
import random
import string
from google_play_scraper import app, exceptions

def generate_code():
    alphanumeric = string.ascii_uppercase + string.digits
    return ''.join(random.choice(alphanumeric) for _ in range(16))

def check_redeemability(codes, country="us"):
    redeemable_codes = []

    for code in codes:
        try:
            result = app(code, lang="en", country=country)
            if result["url"]:
                redeemable_codes.append({"code": code, "url": result["url"]})
                print(f"Code: {code} is redeemable. URL: {result['url']}")
                break  # Stop checking once a redeemable code is found
        except exceptions.NotFoundError:
            print(f"Code {code} is not found on the Google Play Store.")
        except exceptions.ExtraHTTPError:
            print(f"An HTTP error occurred while checking code {code}.")

    return redeemable_codes

# Generate 10 Google Play codes
codes_to_check = [generate_code() for _ in range(10)]

# Change the country code if needed, default is "us" (United States)
country_code = "in"  # Example: "in" for India

redeemable_codes_list = check_redeemability(codes_to_check, country_code)
