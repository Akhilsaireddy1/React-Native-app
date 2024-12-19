# React-Native-app
Designed a React Native App that consists of: A splash screen A Home screen A Search screen A details screen

import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Ionicons } from '@expo/vector-icons';

import HomeScreen from './screens/HomeScreen';
import SearchScreen from './screens/SearchScreen';
import DetailsScreen from './screens/DetailsScreen';

const Tab = createBottomTabNavigator();

const Navigation = () => {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({ route }) => ({
          tabBarIcon: ({ focused, color, size }) => {
            let iconName;

            if (route.name === 'Home') {
              iconName = focused ? 'home' : 'home-outline';
            } else if (route.name === 'Search') {
              iconName = focused ? 'search' : 'search-outline';
            }

            return <Ionicons name={iconName} size={size} color={color} />;
          },
          tabBarActiveTintColor: 'blue',
          tabBarInactiveTintColor: 'gray',
        })}
      >
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Search" component={SearchScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
};

export default Navigation;
3. HomeScreen Implementation:

Create a HomeScreen.js file:
JavaScript

import React, { useState, useEffect } from 'react';
import { FlatList, Text, View, Image, TouchableOpacity, StyleSheet } from 'react-native';

const HomeScreen = ({ navigation }) => {
  const [movies, setMovies] = useState([]);

  useEffect(() => {
    const fetchMovies = async () => {
      try {
        const response = await fetch('https://api.tvmaze.com/search/shows?q=all');
        const data = await response.json();
        setMovies(data);
      } catch (error) {
        console.error('Error fetching movies:', error);
      }
    };

    fetchMovies();
  }, []);

  return (
    <FlatList
      data={movies}
      keyExtractor={(item) => item.show.id.toString()}
      renderItem={({ item }) => (
        <TouchableOpacity
          onPress={() => navigation.navigate('Details', { movie: item.show })}
          style={styles.movieItem}
        >
          <Image
            source={{ uri: item.show.image.medium }}
            style={styles.movieImage}
          />
          <View style={styles.movieDetails}>
            <Text style={styles.movieTitle}>{item.show.name}</Text>
            <Text style={styles.movieSummary}>{item.show.summary}</Text>
          </View>
        </TouchableOpacity>
      )}
      style={styles.container}
    />
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 10,
  },
  movieItem: {
    marginBottom: 20,
  },
  movieImage: {
    width: 150,
    height: 200,
    borderRadius: 10,
  },
  movieDetails: {
    marginLeft: 10,
  },
  movieTitle: {
    fontWeight: 'bold',
  },
  movieSummary: {
    color: 'gray',
  },
});



