---
layout: ../../layouts/MarkdownLayout.astro
title: drinkApp
author: Mirosław Wierzbicki
date: 2025-02-01 08:30
status: finished ✅
repo: https://github.com/miwierzbicki/react-drinkapp
---
## Project: drinkApp - Cocktail Discovery Application

### Overview
The **drinkApp** is a web application developed as a project during a course at the Wrocław University of Technology. This application allows users to explore a vast database of cocktails, retrieving information from the `thecocktaildb.com` API. 

### Core Features

*   **Drink Search Functionality:**
    *   Users can easily search for cocktails by their name using a search bar. The application queries the external API in real-time to fetch matching results.

*   **Interactive Card View:**
    *   Search results and individual drinks are presented in a card layout, providing a quick overview of each cocktail.

*   **Detailed Drink Information:**
    *   Clicking on a specific drink card reveals comprehensive details, including:
        *   **Step-by-step preparation instructions.**
        *   A list of **ingredients** with their respective measures.
        *   The **category or type** of the drink (e.g., alcoholic, non-alcoholic).
        *   The recommended **type of glass** for serving.

*   **Client-Side Routing:**
    *   Implemented using **React Router**, enabling seamless navigation between different views (e.g., search results, detailed drink page) without full page reloads, enhancing the user experience.

### Technologies Utilized

*   **Frontend Framework:** **React** 
*   **Styling:** **Bootstrap** 
*   **API Integration:** Dynamically fetches cocktail data from the public `thecocktaildb.com` API.

### Setup and Installation

To run this project locally:

1.  Clone the repository:
    ```bash
    git clone https://github.com/miwierzbicki/react-drinkapp.git
    ```
2.  cd into the project directory:
    ```bash
    cd react-drinkapp
    ```
3.  Install dependencies:
    ```bash
    npm install
    ```
4.  Start the development server:
    ```bash
    npm start
    ```
