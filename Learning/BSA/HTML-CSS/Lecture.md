### What is HTML?
- HTML language
- There are some elements which doesn't have a closing tag, like `img`.
- Nesting elements

#### HTML Structure
- There are 4 key elements:
	- Document type declaration:
		- It tells the browser what kind of document it's going to work with. 
		- `<!DOCTYPE html>`
	- HTML element:
		- It's the root element
		- Good place to specify the default language for the document 
		- `<html lang="e">`
	- HEAD element:
		- It contains all the metadata about the document.
		- Everything you put there, will not be rendered to the page
#### Block versus Inline elements

| Block elements | Inline elements |
| -------------- | --------------- |
| \<div>         | \<span>         |
| \<ul>          | \<img>          |
| \<table>       | \<input>        |
| \<p>           | \<label>        |
| \<form>        | \<strong>       |
| ...            |                 |
**Formatting**: by default, block-level elements begin on new lines, but inline elements can start anywhere in a line.
**Content model**: generally, block-level elements may contain inline elements and other block-level elements. Inherent in this structural distinction is the idea that block elements create "larger" structures than inline elements.

### Cascade Specificity
![[Pasted image 20240627124716.png]]
### Heritance
- Not all properties can be inherited:
	- border, margin , width, height and padding

### Values and Units
- Absolute units (px, mm, sm, in, pt, pc)
- Relative units (%, em, rem, vh, vw, cx, ch)
- Unitless values
- Colors (key words, hex, rgb(), rgba(), hsl(), hsla())
- Functions

### CSS Box Model
- margin
- border
- padding
- content

```TS
import { getMovieDetailsById } from '../../modules/movies/movies';

import { Movie } from '../types/types';

  

const addFavoriteMovie = (id: number) => {

const favoriteMovies: number[] = JSON.parse(localStorage.getItem('favoriteMovies') ?? '[]');

  

favoriteMovies.push(id);

  

localStorage.setItem('favoriteMovies', JSON.stringify(favoriteMovies));

};

  

const removeFavoriteMovie = (id: number) => {

const favoriteMovies: number[] = JSON.parse(localStorage.getItem('favoriteMovies') ?? '[]');

  

const filteredMovies = favoriteMovies.filter((movieId) => movieId !== id);

  

localStorage.setItem('favoriteMovies', JSON.stringify(filteredMovies));

};

  

const existFavoriteMovie = (id: number) => {

const favoriteMovies: number[] = JSON.parse(localStorage.getItem('favoriteMovies') ?? '[]');

return favoriteMovies.find((movieId) => movieId === id);

};

  

const getFavoriteMoviesIds = (): number[] => {

const favoriteMovies: number[] = JSON.parse(localStorage.getItem('favoriteMovies') ?? '[]');

return favoriteMovies;

};

  

const getMoviesDetailsFromIds = async (moviesIds: number[]): Promise<Movie[]> => {

try {

return await Promise.all(moviesIds.map(async (movieId) => getMovieDetailsById({ id: movieId.toString() })));

} catch (error) {

throw new Error((error as Error).message);

}

};

  

export { addFavoriteMovie, removeFavoriteMovie, existFavoriteMovie, getFavoriteMoviesIds, getMoviesDetailsFromIds };
```

```HTML
```html
<div id="film-container" class="row">
   <div class="col-12 p-2 col-lg-3 col-md-4">
      <div class="card shadow-sm">
         <img src="https://image.tmdb.org/t/p/original//rTh4K5uw9HypmpGslcKd4QfHl93.jpg"> 
         <svg xmlns="http://www.w3.org/2000/svg" stroke="red" fill="red" width="50" height="50" class="bi bi-heart-fill position-absolute p-2" viewBox="0 -2 18 22">
            <path fill-rule="evenodd" d="M8 1.314C12.438-3.248 23.534 4.735 8 15-7.534 4.736 3.562-3.248 8 1.314z"></path>
         </svg>
         <div class="card-body">
            <p class="card-text truncate"></p>
            <div class=" d-flex justify-content-between align-items-center "> <small class="text-muted"></small> </div>
         </div>
      </div>
   </div>
   <div class="col-12 p-2">
      <div class="card shadow-sm">
         <img src="https://image.tmdb.org/t/p/original//rTh4K5uw9HypmpGslcKd4QfHl93.jpg"> 
         <svg xmlns="http://www.w3.org/2000/svg" stroke="red" fill="red" width="50" height="50" class="bi bi-heart-fill position-absolute p-2" viewBox="0 -2 18 22">
            <path fill-rule="evenodd" d="M8 1.314C12.438-3.248 23.534 4.735 8 15-7.534 4.736 3.562-3.248 8 1.314z"></path>
         </svg>
         <div class="card-body">
            <p class="card-text truncate"></p>
            <div class=" d-flex justify-content-between align-items-center "> <small class="text-muted"></small> </div>
         </div>
      </div>
   </div>
   <div class="col-12 p-2">
      <div class="card shadow-sm">
         <img src="https://image.tmdb.org/t/p/original//rTh4K5uw9HypmpGslcKd4QfHl93.jpg"> 
         <svg xmlns="http://www.w3.org/2000/svg" stroke="red" fill="red" width="50" height="50" class="bi bi-heart-fill position-absolute p-2" viewBox="0 -2 18 22">
            <path fill-rule="evenodd" d="M8 1.314C12.438-3.248 23.534 4.735 8 15-7.534 4.736 3.562-3.248 8 1.314z"></path>
         </svg>
         <div class="card-body">
            <p class="card-text truncate"></p>
            <div class=" d-flex justify-content-between align-items-center "> <small class="text-muted"></small> </div>
         </div>
      </div>
   </div>
</div>
```
```
