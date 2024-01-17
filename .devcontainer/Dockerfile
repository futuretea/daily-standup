# Use an official Node runtime as a parent image
ARG NODE_VERSION=18.19.0
FROM node:${NODE_VERSION}-alpine

# Set the working directory in the Docker image
WORKDIR /app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of the project files into the working directory
COPY . .

# Make port 8080 available to the world outside this container
EXPOSE 8080
