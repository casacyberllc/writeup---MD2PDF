# writeup---MD2PDF

<img width="1015" height="289" alt="image" src="https://github.com/user-attachments/assets/32cdc373-1df1-4d6a-a47a-7a9851feec82" />
<hr />

Information: 
- The utility is on a web browser and converts markdown into a downloadable PDF Format
- Given the access IP address
  
<hr />

Tools needed: 
- nmap
- ffuf
- exiftool
- html

<hr />

**Step 1**

Run nmap to see what ports are open on the server. 
`sudo nmap -sS <ipaddress`

I see ports 22 SSH, 80 HTTP and 5000 UPnP are open. I take note of these. 

<hr />

**Step 2**
Lets type the IP address in the web browser to take a look at the website. 

I just see it's a website with a logo header, a textarea and a button to convert the text to PDF. simple enough. No navigation, no nothing. 


<hr />

**Step 3**

Lets check to see if there are any hidden subdomains using:

`ffuf -w /usr/share/../common.txt -u "http://ipaddress/FUZZ"`

<img width="873" height="390" alt="image" src="https://github.com/user-attachments/assets/32c1a042-33b0-4675-a567-8ecfb3aab1ad" />

I see it has an admin page. Lets go check it out and see what is on the page in the web browser. 

<hr />

**Step 4**

The webpage doesn't seem to let us check it out because it's only accessible on an internal network. There has to be a vulnerability that is going to let us access the admin console. I checked the developer tools and source code to see if there but I found nothing interesting. Let's put it to the side for now. 

<hr />

**Step 5**
Let's go back to the main webpage and check out the document that gets created with this website. We're going to enter anything into the textarea, press the button and use `exiftool` to check out the downloaded PDF document. 

**exiftool is used for reading, writing, and manipulating image, audio, video, and PDF metadata.**

Lets download the PDF and then inspect it with the exiftool. 

<hr />

**Step 6**

We got the following information from inspecting with exiftool:

<img width="559" height="334" alt="image" src="https://github.com/user-attachments/assets/49037691-badf-423b-b728-480825610d48" />


From here we're going to pay attention to the part that says **Creator: wkhtmltopdf 0.12.5**

We're going to google this and see what comes up about the tool it's using. We get the following information letting us know it converts HTML to the PDF. Let's see if we can try using HTML injection vulnerability to get access to the admin portal. 

<img width="1008" height="398" alt="image" src="https://github.com/user-attachments/assets/1972133a-201a-4462-b09f-7f7fae224d67" />

<hr />

**Step 7**

Let's go back to the main webpage, and use an iframe tag to try to redirect to our admin console. An iframe tag specifies an inline frame. An inline frame is used to embed another document within the current HTML document.

<img width="1228" height="485" alt="image" src="https://github.com/user-attachments/assets/1c153aeb-5de2-49b2-9f03-753ff5301ac7" />

Let's click convert to PDF and see what happens. 

Tada, we have accessed the admin console and have received the flag for our challenge. 

<img width="1282" height="773" alt="image" src="https://github.com/user-attachments/assets/1c59a872-613f-4751-89ee-8257d78400c7" />

flag{1f4a2b6ffeaf4707c43885d704eaee4b}
