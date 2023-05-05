Download Link: https://assignmentchef.com/product/solved-csi4139-ceg-4399-design-of-secure-computer-systems-laboratory-2
<br>
<strong>Goal</strong>:  Use 2-factor authentication and various cryptographic algorithms to establish a secure channel with a host site.




<strong>Details:  </strong>Create a website that a user logs into using both a keyboard and a cell phone.  Once the user clicks the login button / link at the website, the user is presented with a login screen.  When the user enters his/her username and hits &lt;return&gt;, the website consults a file that it keeps locally to see which cell phone number is associated with that username.  The website then sends an SMS text message to that phone number.  The user, having received the text message, enters it and a password to a local application, which does some processing and returns a string.  The user enters this string at the login screen and hits &lt;return&gt;.  The website validates the sent value; if correct, it returns 2 strings which the user can input to the local application.  The application validates these strings and returns <em>success</em> or <em>fail</em> to the user.




During the lab demonstration of your implementation, the TA will confirm the successful operation of your program.







<strong>Deliverables</strong>:  You are expected to write a document and demonstrate your program.




Document:  Write a brief description (no more than one page) of your program.  In particular, you should describe your programming environment, programming language, and any underlying assumptions about the communications environment.  Along with this, you must include a detailed analysis (approximately two pages) of your implementation.  (You will need to choose encryption, MAC, and signature algorithms, and appropriate key and parameter sizes to balance security, efficiency, and usability.  You must discuss and defend all your choices.)  Finally, include approximately one page discussing any disadvantages that you see with this protocol.




Software: You must implement the software to do the protocol described on the following page.  This will consist primarily of code to send and receive the appropriate messages, as well as a user interface sufficient to allow the TA to observe what your program is doing.  (<em>Note:  if you wish, you can do everything on Alice’s side on her phone, rather than on her computer &amp; phone.</em>)







<em>This laboratory project is to be done in groups of up to 3 people. </em>

Protocol Description




System parameters (known to everyone) are a prime number <em>p</em> and a generator <em>g</em> of ℤ<em><sub>p</sub></em><sup>*</sup>.  At initialization time, Alice chooses password <em>a</em>, computes α = <em>g<sup>a</sup></em> mod <em>p</em>, and gives α to the host, who stores it along with Alice’s username and cell phone number.  The host gives its signature verification key to Alice, who stores this key as a trust anchor.




At login time:

<ul>

 <li>Alice sends her username (“Alice”) to the host, Bob.</li>

 <li>Bob looks up α, chooses (at random) a value <em>b</em>, and computes β = <em>g<sup>b</sup></em> mod <em>p</em>. Bob sends β to Alice’s cell phone as an SMS text message.</li>

 <li>At her local application, Alice enters β and her password <em>a</em>. The application computes <em>K</em> = β<em><sup>a</sup></em> mod <em>p</em> = <em>g<sup>ab</sup></em> mod <em>p</em>.  The first <em>n</em> bits of <em>K</em> are <em>k</em><sub>1</sub>; the next <em>n</em> bits of <em>K</em> are <em>k</em><sub>2</sub>.  Let <em>m</em> = α || β.  The application computes <em>m</em><sub>1</sub> = E<em><sub>k</sub></em><sub>2</sub>(<em>m</em> || MAC<em><sub>k</sub></em><sub>1</sub>(<em>m</em>)) and displays the result to Alice.  Alice sends this value to the host through the login screen.</li>

 <li>Bob computes <em>K</em> = α<em><sup>b</sup></em> mod <em>p</em> = <em>g<sup>ab</sup></em> mod <em>p</em>. Bob then uses <em>k</em><sub>1</sub> and <em>k</em><sub>2</sub> to validate <em>m</em><sub>1</sub>, constructs <em>m’</em> = β || α, computes <em>m</em><sub>2</sub> = E<em><sub>k</sub></em><sub>2</sub>(<em>m’</em> || MAC<em><sub>k</sub></em><sub>1</sub>(<em>m’</em>)), digitally signs <em>m</em><sub>2</sub> using its private key, and sends {<em>m</em><sub>2</sub>, sig(<em>m</em><sub>2</sub>)} to Alice’s login screen.</li>

 <li>Alice enters {<em>m</em><sub>2</sub>, sig(<em>m</em><sub>2</sub>)} to her local application, which validates <em>m</em><sub>2</sub>, verifies the signature, and returns <em>success</em> or <em>fail</em>.</li>

</ul>




After the successful completion of this protocol, for all subsequent data traffic between Alice and the host, all messages can be MAC’ed using key <em>k</em><sub>1</sub> (for authenticity) and all sensitive messages can be encrypted using key <em>k</em><sub>2</sub> (for confidentiality).







This protocol provides mutual authentication between Alice and the host, Bob (using 2factor authentication for Alice), key establishment, key confirmation, and secure channel establishment.  Alice only needs to remember her password, and there is no need for a trusted third party (e.g., a Certification Authority, CA).


