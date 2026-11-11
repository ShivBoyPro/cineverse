# Cineverse

**Find your next film by exploring how movies connect.**

[Explore Cineverse →](https://explorecineverse.pages.dev/)

![Cineverse movie map](assets/movie-map.png)

Cineverse is an interactive movie map with more than 67,000 feature films. Explore clusters of related stories, search for a title or filmmaker, and move from the big picture into individual films.

## What you can do

- Explore a movie map that opens flat and reveals its depth as you zoom in.
- Search for films, actors, directors, composers, genres and themes.
- Discover related films, with attention to language, story and release era.
- Compare ratings and audience vote counts.
- View posters, film details and U.S. watch options.
- Mark films as watched to keep them out of future recommendations.
- Explore on desktop or phone.

![Film details and related recommendations](assets/film-details.png)

## How I built it

### Turning movie information into a map

I brought together movie metadata from IMDb and TMDB, matched records across sources, and filtered the catalogue to narrative feature films released after 1965. The catalogue excludes shorts, documentaries, series and adult titles.

I represented films using a combination of genre, story keywords, available synopses and creative credits. Those metadata features became numerical vectors, allowing films to be compared by shared characteristics. I then used dimensionality reduction and clustering to organize the catalogue into visual neighborhoods.

The map uses UMAP-based projections and a 3D layout built around those neighborhoods. Movie positions are stored rather than randomly regenerated each time the page opens. Display coordinates support exploration; recommendation ranking uses richer metadata relationships.

### Rendering an interactive 3D map

I built the interface with JavaScript and used Three.js/WebGL to render the movie points in batches. Rendering many points together, limiting the visible detail at a distance, and caching screen-space hit testing help keep navigation responsive.

The opening view uses a narrow perspective and a front-facing camera to appear nearly flat. Selecting a movie or cluster transitions into a stronger perspective, revealing depth. The movie coordinates stay in place throughout that transition.

Genre colors distinguish neighborhoods. Small visual dots have larger invisible interaction areas, making them easier to select without enlarging the map itself. Nested cluster navigation lets users move from broad groups toward individual films.

### Finding related films

I separated relevance from rating quality. Shared story information, genre, people, language and release era inform which films are related. Ratings and vote counts help assess confidence and order results.

Recommendations account for language and regional context, so a thematic match does not automatically override those differences. Audience confidence is considered within language and rating-source contexts, helping avoid both low-vote outliers and a single global threshold that would overlook regional films. Release era also influences ranking.

Browsing a recommended film preserves the original recommendation list until the user explicitly chooses to explore films like the new selection.

### Search and discovery

I built search for titles, genres and credited people, including actors, directors and composers. Normalized text, partial matching and typo tolerance help with misspellings and incomplete queries. As suggestions change, camera previews follow the leading result.

Genre and person searches can open a ranked list alongside the existing map. This version uses metadata-based discovery; it does not offer a conversational AI or free-form semantic description search.

### Posters, details and availability

Live movie details and U.S. watch options come through a server-side API layer. API credentials stay on the server.

Poster loading uses verified API-derived paths, browser caching and prefetching based on likely next selections. Separating poster lookup from the wait for full details reduces loading delays. Request tracking prevents an older response from replacing the poster for a newly selected film.

Missing metadata is shown as unavailable rather than invented. Streaming availability is attributed to JustWatch via TMDB.

### Personal state and deployment

Watched films are stored in the browser and excluded from recommendation lists while remaining searchable. Users can undo that choice. This currently does not sync between devices.

The site is hosted on Cloudflare Pages, with server-side functions handling live metadata requests. Prebuilt, compressed catalogue files support the public map, while the source data processing and implementation remain outside this repository.

I check desktop and phone behavior, nested zoom, click-versus-drag handling, poster-loading races and watched-state persistence. Layout checks verify that rendering changes do not unintentionally move movies.

### Tools used

| Area | Tools |
| --- | --- |
| Data preparation and relationships | Python, NumPy, SciPy, scikit-learn |
| Projection and clustering | UMAP, HDBSCAN |
| Interface and rendering | JavaScript, HTML/CSS, Three.js, WebGL |
| Movie information | IMDb datasets, TMDB API |
| Hosting and server-side requests | Cloudflare Pages and Pages Functions |
| Browser verification | Playwright |

This overview explains the engineering approach. Source code, exact weighting and ranking rules, processing scripts, datasets and deployment configuration are not included.

## Try it

Open **[explorecineverse.pages.dev](https://explorecineverse.pages.dev/)**. Search for a film you know, select it, and explore its recommendations or nearby clusters.

Watched selections are saved in your browser. Movie metadata and streaming availability may vary by title.

## About this repository

This is a public project showcase. Cineverse's implementation, data pipelines, recommendation logic and deployment configuration are maintained privately.

Movie information includes data from IMDb and TMDB. Watch availability is provided by JustWatch via TMDB. Cineverse is not endorsed or certified by TMDB. Movie posters and third-party materials belong to their respective owners.
