## Docker Container...


So... What is a Docker Container? Hmm?? How is the word `Container` meant??? <br>

To simplify what a Docker Container is - it is a `Virtual Machine`.<br>

Here is a good definition: <br>
 
A `Docker Container` is a *lightweight*, portable, and self-sufficient unit that encapsulates an application and its dependencies. Unlike traditional virtual machines (VMs), which include an entire operating system, Docker containers share the `host` *OS* `kernel` while keeping applications isolated from each other. This makes them more efficient and faster than VMs. <br>

Containers provide a consistent runtime environment across different systems, meaning an application will behave the same whether it's rimming on a developer's laptop, a test server, or a production cloud environment. <br>

I like to think of using VMs as similar to dual-booting operating systems. With new high-tech laptops and multiple monitors, you can run multiple VMs with different operating systems doing what have you, while your main operating system can still function and run processes in parallel. <br>

While I was in college and was learning about various cyber attacks and blue-team procedures, a lot of the training was done using Virtual Machines (VMs). <br> 

### Why build a Container?

Well, answering those questions requires how well I can build a `Container (VM)`. I need to be able to replicate a custom container that is influenced by my curiosity about the use of `Containers` in the Security Industry. <br>

#### How `Containers` Are Used in the `Security Industry`?

In cybersecurity, containers have various use cases:<br>

Application Sandboxing – Containers isolate applications to prevent them from affecting the host system or other applications. If an app is compromised, the attacker remains confined within the container. <br>

Penetration Testing – Security professionals use prebuilt containerized tools like Kali Linux and Metasploit for ethical hacking, vulnerability scanning, and penetration testing. <br>

Threat Detection and Honeypots – Containers can be used to log and analyze unauthorized access attempts, acting as honeypots to lure attackers into monitored environments. <br>

Immutable Infrastructure – Containers are ephemeral, meaning they can be redeployed quickly if a security issue arises, reducing the risk of persistent threats. <br>

Secure Microservices – By breaking applications into smaller containerized components, security teams can enforce stricter access controls, reducing the attack surface. <br>


## Creating the `Docker` Project Directory


Path: `"C:\Users\natha\.docker\myDockerApp"` <br>



These are the `Steps` I took: <br>

In the *docker.desktop* (personal) environment: <br>
find the `>_` *terminal* icon, *click it* to open the terminal. <br>

Step 1. In the Docker terminal enter: <br>

```
mkdir MyDockerApp
cd MyDockerApp
```

next...<br>

`New-Item -Path . -Name "app.py" -ItemType "file"`<br>

Then open `app.py` in `VS Code` and input the following, and don't forget to `ctr + s`<br>

`print("Hello from Docker!")` <br>

Step 2. Create the Dockerfile <br>

In the Docker terminal enter: <br>


`New-Item -Path . -Name "Dockerfile" -ItemType "file"` <br>


Next in `VS code` enter the following...<br>


```
# Use the official Python image
FROM python:3.9

# Set the working directory inside the container
WORKDIR /app

# Copy the Python script into the container
COPY app.py .

# Define the command to run the script
CMD ["python", "app.py"]

```

Don't forget to `ctr + s`<br>

Step 3. Build the `Docker Image` <br>

In the Docker terminal enter: <br>

`docker build -t mydockerapp . ` 

- `-t mydockerapp` -> Names the image "mydockerapp".<br>
- `.` -> Tells Docker to look for the `Dockerfile` in the current directory. <br>


Next enter:<br>

`docker images` <br>

Step 4. *Run* the `Docker Container`

In the Docker terminal enter: <br>

`docker run mydockerapp` <br>

After you press enter you should see: "Hello from Docker!" <br>


________________________________________________________________________________

## Completing the Basics

Now that we have the basics configured 

































