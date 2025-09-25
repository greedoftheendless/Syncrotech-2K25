FROM docker.io/library/ubuntu:latest

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
ENV FLAG4="CTF{3nv1r0nm3nt_v4r_fl4g}"

# Set up a non-root user for the challenge (good practice)
RUN useradd -m ctfuser
USER ctfuser
WORKDIR /home/ctfuser

# Keep the container running
CMD ["tail", "-f", "/dev/null"]