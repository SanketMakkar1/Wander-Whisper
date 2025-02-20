# Wander Whisper
Group 1's Senior Project

Input

User Uploaded Images: \
	Image 1: A photo of a quiet, tropical beach with palm trees.\
	Image 2: A sunset over a mountain range.

User Text Input: \
	I am departing from New York, USA in August and will return in March. My budget is ultra-luxury travel $1000+ per day, and I prefer street food. I will be traveling partner for one month, and I enjoy camping. I prefer a remote village destination with cold weather. My transportation preference is public transport. My trip is in spring, and I am interested in F1 races. The local language should be French-speaking, and I need a electronic travel authorization destination. My trip should be n, and my adventure level is extreme. For nightlife, I prefer beach parties. I prefer locations with low-crime area and service dog friendly support. I am interested in a cultural experience with sustainable tourism focus. I will also be adding guided city tours to my trip.
	
### Processing Steps \
Step 1: Process images: \
	Extract image embeddings folder: image_embeddings_extraction.py -> return a csv image_embeddings.csv (512,)

Step 2: Process user text prompts: \
	Generate 100k synthetic prompts to fine tune the T5 model to generate raw extracted criteria \
	Output from LLM then go through evaluate.py -> clean user_preference.json \
	Extract the embedding from the user_preference.json \

Step 3: Create multi_modal_user_embedding = $\alpha$ * image_embeddings + $\beta$ * text_embeddings \
($\alpha$ and $\beta$ hyperparameter deciding the relative importance of images versus text prompts)

Step 3: Query Knowledge Base

	Visual Matching:
    	Match image embeddings with database image features:
        	Top matches: Bali (Indonesia), Phuket (Thailand), and Huaraz (Peru).
	Textual Matching:
    	Match keywords like "peaceful beach," "scenic views," "good weather," and "March":
        	Filtered destinations: Bali, Phuket.
	Final Match:
    	Combine visual and textual scores, filter by budget and weather:
        	Final recommendation: Bali, Indonesia.

Step 4: Fetch Live Data

	Use APIs to fetch additional dynamic information:
    	Cheapest Flight: $750 round trip (via Skyscanner).
    	Hotel: $70 per night for a beachfront villa (via Booking.com).
    	Weather: Sunny, 27°C average in March (via OpenWeatherMap).

Step 5: LLM-Generated Explanation

	Input Prompt to LLM:

	Based on the user's budget of $2000, preference for a peaceful beach, scenic views, hiking opportunities, and good weather in March, explain why Bali is an ideal vacation spot. Highlight its unique features, such as beaches, restaurants, and photography spots, and how it aligns with their budget.

	Generated Output: "Bali is an excellent destination for your relaxing vacation under $2000. With tranquil beaches like Nusa Dua and scenic mountain views in Ubud, Bali offers the perfect blend of relaxation and adventure. The weather in March is ideal, with sunny days averaging 27°C. Bali also caters to food lovers with a variety of beachfront restaurants serving delicious local and international cuisines. Affordable hotels and flights make Bali a budget-friendly choice, while hiking trails and picturesque landscapes make it a paradise for photography enthusiasts."

Step 6: Curate Images of the Destination

	Retrieve high-quality images of Bali:
    	Beachfront at Nusa Dua.
    	Sunset at Uluwatu Temple.
    	Lush green rice terraces in Ubud.

UI: after the user input, make a world map, where there are several red dots that users can hover, when they hover to that red dot, information about the location appears: images, link to the cheapest flight (maybe SkyScanner API), current weather…., and maybe a suggested tour.
We can also try to extract whether they are travelling alone, couple, family… -> suggest tour accordingly
