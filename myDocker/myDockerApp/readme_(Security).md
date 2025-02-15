## Docker Container Security...

Next...<br>
So, I think what I want to learn from building containers is, when building these containers - I realized that I am, in a way building a custom Linux operating system (and I like me some Linux). My idea is: let's say you start a new position at work or remote work. As you start work with company A - you use a preconfigured docker container. Now, I have no idea if this is logical in the real world, so there that...
This Docker Container is preconfigured to only allow internet traffic from your use - all other incoming traffic is blocked (verify my statement is correct) The user can download if necessary (This does cause an issue).
I want to also allow the user of the container to be able to download and set up the container however they want in regards to their work. So, I still have more reading to do...

I want to create a *safe* & *secure* `container`

I will start with what might be consider best practices.<br>
I will attempt to allow for further augmentations if further research was conducted.<br>

### Use a `Non-Root` User <br>

1. Modify the `Dockerfile` to avoid running as `root`:

```
# Add a new user
RUN useradd -m appuser

# Switch to non-root user
USER appuser
```

2. Reduce Container Privileges:

    - You want to limit the privilages, enter the following: <br>

` docker run --read-only --memory=128 --cpus=0.5 mydockerapp ` <br>

- `--read-only`  -> Makes the filessystem immutable. <br>
- `--memory=128` -> Limits memory usage. <br>
- `--cpu=0.5`    -> Limits CPU consumption. <br>

3. `Enable` Logging for *Unauthorized* Access

...Thoughts...
ok, so this is where `Docker` kind of blows my mind. I want to create a secure platform, which is what the `Docker Container` can achieve. But because it is Linux-based, I can implement security bash scripts and custom firewalls.?. Think about all the prevention tools!!! YO!<br>

This is going to be fun!!! <br>

` New-Item -Path . -Name "monitor.sh" -ItemType "file" ` <br>
