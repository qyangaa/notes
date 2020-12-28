```jsx
import React, { Component } from 'react';
import { getMovies } from "../services/fakeMovieService";
import 'bootstrap/dist/css/bootstrap.css';
import 'font-awesome/css/font-awesome.css'

class Movies extends Component {
    state = {  
        movies: getMovies()
    };

    constructor(){
        super();
        this.renderMovie.bind(this);
        this.renderDescription.bind(this);
    }

    renderMovie(movie){
        return (
            <tr key={movie._id}>
                <td>{movie.title}</td>
                <td>{movie.genre.name} </td>
                <td>{movie.numberInStock}</td>
                <td>{movie.dailyRentalRate}</td>
                <td>
                    <button onClick={()=> this.handleDelete(movie)} className="btn btn-danger btn-sm">Delete</button>
                </td>
            </tr>
        )
    }

    handleDelete(targetMovie){
        const filteredMovies = this.state.movies.filter(movie=> movie!=targetMovie)
        this.setState({movies: filteredMovies})
    }

    renderDescription(){
        const length = this.state.movies.length;
        if(length>0){
            return <p>Showing {length} movies in the database.</p>
        }else{
            return <p>No movie to show</p>
        }
    }

    render() { 
        return ( 
            <React.Fragment>
                {this.renderDescription()}
                <table className="table">
                    <thead>
                        <tr>
                            <th scope="col"> Title </th>
                            <th scope="col"> Genre </th>
                            <th scope="col"> Stock </th>
                            <th scope="col"> Rate </th>
                            <th />
                        </tr>
                    </thead>
                    <tbody>
                        {this.state.movies.map(movie => this.renderMovie(movie))}
                    </tbody>
                </table>
            </React.Fragment>
        );
    }
}

export default Movies;
```

