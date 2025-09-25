# ---- Builder Stage (Keep this part of the Dockerfile secret) ----
FROM docker.io/library/ubuntu:latest as builder

# Install necessary tools
RUN apt-get update && apt-get install -y \
    zip \
    unzip \
    john \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Create challenge directories and files
WORKDIR /ctf

# Flag 1: Hidden file in a hidden directory
RUN mkdir .hidden_flags && echo "CTF{h1dd3n_f1l3_fl4g}" > .hidden_flags/flag1.txt

# Flag 2: Restricted permissions
RUN mkdir flag2_dir && echo "CTF{p3rm1ss10n_d3n13d_fl4g}" > flag2_dir/flag2.txt && chmod 400 flag2_dir/flag2.txt

# Flag 3: Password-protected ZIP
RUN echo "CTF{z1p_p4ssw0rd_fl4g}" > flag3.txt
RUN zip --password "easy" flag3_archive.zip flag3.txt
RUN rm flag3.txt

# Flag 4: Environment variable
# We write the environment variable to a file that will be sourced on login.
RUN echo 'export FLAG4="CTF{3nv1r0nm3nt_v4r_fl4g}"' > /etc/profile.d/ctf_env.sh


# ---- Student Stage (This is what you share with your students) ----
FROM docker.io/library/ubuntu:latest

# We need to install sudo again because this is a new stage.
RUN apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*

# Copy the challenge files from the builder stage.
# This copies the files without revealing how they were created.
COPY --from=builder /ctf /ctf
COPY --from=builder /etc/profile.d/ctf_env.sh /etc/profile.d/ctf_env.sh

# Set up a non-root user for the challenge
RUN useradd -m ctfuser
USER ctfuser
WORKDIR /home/ctfuser

# Start a shell for the user to interact with the challenge
CMD ["bash"]
