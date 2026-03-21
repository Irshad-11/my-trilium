
<p align="center">
  <h1 align="center"><a href="https://irshad-11.github.io/my-trilium/">My Trilium</a></h1>
  <p align="center"><b>A High-Speed Self-Hosted <a href="https://github.com/TriliumNext/Trilium"><i>Trilium Server</i></a> Running at $0 Infrastructure Cost</b></p>
</p>
<div align="center">

<img src="https://img.shields.io/badge/Trilium-Server-1a1a1a?style=flat&logo=bookstack&logoColor=white&labelColor=0f0f0f">
<img src="https://img.shields.io/badge/Self-Hosted-202020?style=flat&logo=linux&logoColor=white&labelColor=141414">
<img src="https://img.shields.io/badge/Zero-Cost-262626?style=flat&logo=opensourceinitiative&logoColor=white&labelColor=181818">
<img src="https://img.shields.io/badge/Cloudflare-Tunnel-2c2c2c?style=flat&logo=cloudflare&logoColor=white&labelColor=1c1c1c">
<img src="https://img.shields.io/badge/Auto-Reconnect-323232?style=flat&logo=githubactions&logoColor=white&labelColor=202020">
<img src="https://img.shields.io/badge/Page-Redirect-383838?style=flat&logo=github&logoColor=white&labelColor=242424">

<br>

![Visitors](https://visitor-badge.laobi.icu/badge?page_id=Irshad-11.my-trilium&left_color=1a1a1a&right_color=2a2a2a)

</div>

---



## THE CONTEXT

This project is basically a **solution approach for a very specific problem**: **How can I host a Trilium Notes server on the internet completely free with $0 infrastructure cost?**

<img src="https://substackcdn.com/image/fetch/$s_!86eO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F806be787-874e-43e1-85d2-2885ef07f4d6_128x128.png" align="right" width="150" />





Most self-hosting guides assume that the user already has:

- a purchased domain  
- a cloud VPS  
- a static public server  

But in reality many students, researchers, and personal users simply want to access their **own notes remotely** without paying monthly infrastructure costs.

This repository documents my **working solution architecture** for that problem.

The approach combines several free tools and automation ideas:

- Local Trilium server (`localhost`)
- Cloudflare quick tunnels
- Git automation
- GitHub Pages redirect

Together they create a **permanent public entry point** that always redirects to the current active server tunnel.

The system automatically adapts when:

- WiFi disconnects
- the local machine goes offline
- the tunnel URL changes
- the server restarts

Everything reconnects automatically and the redirect link updates itself.

And most importantly:

> **The entire infrastructure runs with $0 cost.**


## What is it

This project is basically my personal **Trilium Notes server** running with **ZERO infrastructure cost**.  
The system presented here is not the first idea — it is the **final working approach after several failed attempts and architectural experiments**.

[Trilium Notes](https://github.com/TriliumNext/Trilium) is a powerful and widely used **knowledge management system**. It allows users to create structured notes, connect ideas through links, build hierarchical knowledge trees, and maintain a personal knowledge base that grows over time.

By default, Trilium works perfectly as a **local knowledge system**. Everything runs on your computer, and your data stays completely under your control. However, Trilium also supports **self-hosted server synchronization**, which means the notes can be accessed remotely from other devices such as phones, tablets, or other computers.

Normally when people deploy Trilium remotely they use:

- A cloud server (VPS)
- A purchased domain
- Online storage infrastructure

While this works well, it introduces **cost and maintenance responsibilities**.

For a personal project — especially as a student — this infrastructure becomes unnecessary. Paying for domains and cloud servers just to access personal notes remotely does not make practical sense.

Because of that, the goal of this project became very clear:

> **Access my local Trilium Notes from anywhere in the world without paying a single dollar.**

After several experiments, failures, and redesigns, the system finally reached a working architecture.


## What my Goals

Before building the system I defined a few important goals.

First, I wanted the ability to **access my local Trilium Notes remotely from anywhere in the world**.  
Whether using my phone, another laptop, or another network, the notes should always remain accessible.

Second, the system needed to be **self-healing**.

If my local machine temporarily goes offline — for example when WiFi disconnects — the system should automatically recover once the internet connection returns. I did not want a fragile system where I have to manually restart services every time the network fails.

Third, and most importantly:

> The entire system must operate with **ZERO COST infrastructure**.

That means:

- No paid domains
- No VPS hosting
- No cloud storage subscriptions
- No recurring services

Everything must rely only on **free tools and open infrastructure**.


## How it Starts

Trilium includes a feature called **Server Synchronization**.  
This allows multiple Trilium clients to sync notes with a server instance.

If someone already owns a domain and a cloud server, the setup is extremely easy.  
The server can simply be exposed through a tunnel or reverse proxy and accessed remotely.

However, my challenge was different:

> **Build the entire system without buying a domain or paying for a server.**

So the process started with several attempts.


## First Try

My first approach was the simplest one.

Using **Cloudflare Quick Tunnel**, I exposed my local Trilium server to the internet. This allowed me to access the server through a public URL generated by Cloudflare.

Initially this worked very well.

I could open the URL from anywhere and the Trilium interface loaded perfectly.

However, there was a major limitation.

The URL generated by the Quick Tunnel was **random each time the tunnel started**.

Every restart created a new public address.

This meant I constantly needed to update the server configuration with the new URL.

But the real problem appeared when my local computer lost internet connection.

If WiFi disconnected:

- The tunnel stopped
- Remote access stopped working
- The server became unreachable

Even when the WiFi came back, the tunnel **did not restart automatically**.

I had to manually start the entire system again.

That defeated the goal of having an **automatic resilient system**.


## Second Try

The next attempt focused on obtaining a **free domain or subdomain**.

If I could attach a stable domain to the tunnel, the changing URL problem would disappear.

I experimented with several free domain providers and dynamic DNS services.

Although I was able to obtain subdomains, another limitation appeared.

Most free subdomains **cannot be fully registered inside Cloudflare’s DNS system**. Cloudflare allows domains to be added easily, but external free subdomains usually cannot be migrated completely.

Because of that, the tunnel authorization process failed.

So this attempt also ended unsuccessfully.



## Third Try

After more searching, I discovered **LocalTunnel**.

LocalTunnel provides a **static public URL for local services**, and it is completely free.

At first this looked like the perfect solution.

The URL remained stable and did not change between restarts.

Unfortunately, the performance was extremely poor.

The connection latency was high and loading the Trilium interface took too long. For a system that I planned to use daily, the slow speed made it impractical.

Technically it worked — but practically it was unusable.


## Fourth Try

The next attempt involved **deSEC DNS combined with Cloudflare Named Tunnels**.

Cloudflare Named Tunnels are powerful because they provide **permanent addresses** rather than random temporary ones.

However there was another limitation.

Cloudflare requires the domain to be managed through its own nameservers. Free dynamic subdomains such as `.dedyn.io` cannot be fully transferred into Cloudflare infrastructure.

Because of that restriction, the authentication process failed again.

Another attempt ended in failure.


## Final Try

After multiple failures I realized something important.

Instead of trying to **stop the tunnel URL from changing**, I should simply **adapt to the change automatically**.

So the architecture changed completely.

The final system works like this:

- The Trilium server runs locally on `localhost:8080`.
- A Cloudflare Quick Tunnel exposes the server to the internet.
- Each time the tunnel starts, it generates a **new random URL**.
- A script automatically detects that URL.
- The script commits the URL into a Git repository.
- A GitHub Pages site redirects users to the newest tunnel address.

Now instead of remembering the changing tunnel link, I only visit:

```
irshad-11.github.io/my-trilium
````

This page always redirects to the **latest active tunnel URL**.

Even better, the system survives network interruptions.

If the local machine goes offline:

1. The tunnel stops
2. The script detects internet loss
3. It waits for connectivity

Once WiFi returns:

1. The tunnel restarts
2. A new public URL is generated
3. The repository automatically updates
4. GitHub Pages redirects to the new server

To verify the system status, I simply check the **Git commit history**.  
If a new commit appears, it means the server restarted successfully and the new URL has been published.

Through this architecture, the entire system works **without paying for any infrastructure**.

---

## Trilium.sh Command

```bash
#!/bin/bash

# This script runs my Trilium server and automatically exposes it to the internet.
# I personally run this on Ubuntu Linux.
# If someone is using Windows, they can still run this script using WSL, Git Bash, or any Linux-like shell.

# --- USER PATH SETTINGS ---
# Location of the Trilium server installation
TRILIUM_PATH=~/TriliumNotes-Server-0.102.0-linux-x64/

# Local path of the GitHub repository that hosts the redirect page
REPO_PATH="/mnt/8A001FF4001FE64B/**<YOUR_PATH>**/my-trilium"
# ---------------------------


# Start the Trilium server locally
# It runs in background so the script can continue working
cd "$TRILIUM_PATH"
./trilium.sh --listen 0.0.0.0 --port 8080 &

echo "Trilium Server started in background."


# Main loop of the system
# This loop continuously monitors internet connection
# and maintains the tunnel + GitHub redirect automatically
while true; do

    # Check internet connectivity by pinging Google DNS
    if ping -q -c 1 -W 1 8.8.8.8 >/dev/null; then

        echo "$(date): Internet connection detected. Starting Cloudflare tunnel..."

        # Start Cloudflare quick tunnel
        # The tunnel exposes localhost:8080 to the internet
        # Output logs are saved temporarily so we can extract the public URL
        cloudflared tunnel --url http://localhost:8080 > /tmp/tunnel.log 2>&1 &
        TUNNEL_PID=$!

        # Wait a few seconds so Cloudflare can generate the public URL
        sleep 10

        # Extract the generated tunnel URL from log output
        NEW_URL=$(grep -o 'https://[-0-9a-z]*\.trycloudflare\.com' /tmp/tunnel.log | head -n 1)

        if [ ! -z "$NEW_URL" ]; then

            echo "SUCCESS: Tunnel URL generated -> $NEW_URL"

            # Update GitHub redirect page
            # This page automatically redirects visitors to the current tunnel address
            echo "<!DOCTYPE html><html><head><meta http-equiv='refresh' content='0; url=$NEW_URL'></head><body style='background:#1a1a1a;color:white;text-align:center;padding-top:50px;'><h2>Redirecting to Trilium...</h2><script>window.location.href='$NEW_URL';</script></body></html>" > "$REPO_PATH/index.html"

            # Commit and push the new redirect page
            # This updates the GitHub Pages site with the latest tunnel URL
            cd "$REPO_PATH"
            git add index.html
            git commit -m "Auto-Reconnect: $(date)"
            git push origin main

            echo "GitHub redirect page updated."

            # Monitoring phase
            # As long as internet works and tunnel process is alive
            # the script will simply wait and check every 30 seconds
            while ping -q -c 1 -W 1 8.8.8.8 >/dev/null && ps -p $TUNNEL_PID > /dev/null; do
                sleep 30
            done

            echo "Connection lost or tunnel stopped. Preparing restart..."
        fi

        # Stop the tunnel process before restarting again
        kill $TUNNEL_PID 2>/dev/null

    else
        # If internet is not available the script waits
        echo "$(date): Internet not available. Waiting 10 seconds..."
    fi

    sleep 10
done
````


## Explore Trilium Notes

If you have never used **Trilium Notes**, it is absolutely worth exploring.

Trilium is not just a simple note application.  
It is a **powerful personal knowledge management system** designed for organizing complex information.

Some things that make Trilium amazing:

- hierarchical knowledge trees
- deep internal linking between notes
- scripting and automation support
- powerful search and navigation
- full control of your data
- completely self-hostable

You can learn more or try it yourself here:

**Trilium Repository:**  
https://github.com/TriliumNext/Trilium

If you enjoy building structured knowledge bases, research notebooks, or personal documentation systems — Trilium is an incredibly powerful tool.

## Attention

This project is not meant to replace professional infrastructure.

Instead, it demonstrates that **creative engineering and automation can solve real problems even with zero budget**.

If you are a student, developer, or self-hosting enthusiast trying to run your own tools without paying for cloud services — this approach might be useful.

Feel free to explore the repository, modify the script, or adapt the idea for your own projects.



## It's Me

**Irshad Hossain**

Thank you for visiting.

