## Day 2 – Temporary User Setup with Expiry

To create a temporary user account that expires automatically after a certain date:

Here rahul is the temporary user

```bash
# Create user with expiry date (YYYY-MM-DD format)
sudo useradd -m -e 2025-12-31 rahul

# To verify the expiry do this 
sudo chage -l rahul

# Output will show like this:
Account expires    : Dec 31, 2025
