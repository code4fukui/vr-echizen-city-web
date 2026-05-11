# VR鯖江ツクリテ辞典 (VR Sabae Creator's Dictionary)

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

A web-based VR experience showcasing creators from Sabae City, Japan. This project uses A-Frame to display artisans' photos and biographical data on two-sided, rotating panels arranged in a cylindrical layout.

## Demo

- [Live Demo](https://code4fukui.github.io/vr-scc-creators/)

## Features

-   **Immersive VR Gallery**: Displays creator profiles in a 3D space using A-Frame.
-   **Two-Sided Panels**: Each panel shows a creator's photo on the front and detailed text information on the back.
-   **Dynamic Image Generation**: Automatically converts structured text data into image textures for the back panels using HTML-to-Canvas conversion.
-   **Local Caching**: Includes a Deno server to generate and save the text-based images to a `data/` directory on first run, ensuring fast load times for subsequent views.
-   **Circular Animation**: Panels are arranged in a multi-layered cylinder and rotate slowly for a dynamic browsing experience.

## How It Works

The application fetches creator data from a public CSV file. For each creator, it creates a pair of back-to-back `a-plane` entities in A-Frame.

1.  **Image Generation (First Run)**: When the `maketextimg` flag in `index.html` is set to `true`, the `creators.js` script dynamically creates an HTML element with the creator's data, converts it to a PNG image, and sends the Base64-encoded data to a local Deno server.
2.  **Caching**: The `make_textimg_server.js` server receives the data, decodes it, and saves it as a `.png` file in the `data/` directory.
3.  **Normal Operation**: When `maketextimg` is `false`, the application loads the pre-generated images directly from the `data/` directory, skipping the generation step.

## Setup and Usage

This project has a two-step setup process: generating the text images once, and then running the main application.

### Step 1: Generate Text Images (First-Time Setup)

This process uses a local Deno server to convert the creator data into images and save them locally.

1.  Clone the repository.
2.  In `index.html`, set the `maketextimg` constant to `true`:
    ```javascript
    const maketextimg = true; // if true, run with make_textimg_server.js
    ```
3.  Start the Deno server from your terminal. This server will listen for and save the generated images.
    ```sh
    deno run --allow-net --allow-write make_textimg_server.js
    ```
4.  Open `index.html` in a web browser. The page will load, and you should see image files being created in a new `data/` directory in your project folder. Once all panels are visible, this step is complete.

### Step 2: View the Experience

Once the images are cached in the `data/` directory, you can run the experience without the Deno server.

1.  Stop the Deno server (Ctrl+C).
2.  In `index.html`, set the `maketextimg` constant back to `false`:
    ```javascript
    const maketextimg = false;
    ```
3.  Serve the project folder using any local web server and open `index.html` in your browser.

## Data Source

This project uses open data provided by the Sabae Chamber of Commerce and Industry (SCC).

-   [SCC ツクリテ2022 - 職人辞典オープンデータ](https://github.com/code4fukui/scc_creators_opendata/)
