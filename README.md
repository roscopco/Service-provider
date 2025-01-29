# Service-provider
Simple Search Functionality: Users can input what service they need (e.g., "dog walker," "home cleaning") and find providers near them.
import React, { useState } from 'react';
import { Search, MapPin } from 'lucide-react';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";

// Mock data for service providers
const mockProviders = [
  {
    id: 1,
    name: "John's Dog Walking",
    service: "dog walker",
    rating: 4.8,
    location: "Downtown",
    distance: "0.5 miles",
    price: "$20/hour"
  },
  {
    id: 2,
    name: "Clean Home Pro",
    service: "home cleaning",
    rating: 4.6,
    location: "Westside",
    distance: "1.2 miles",
    price: "$25/hour"
  },
  {
    id: 3,
    name: "Happy Paws Pet Care",
    service: "dog walker",
    rating: 4.9,
    location: "Northside",
    distance: "0.8 miles",
    price: "$22/hour"
  },
  {
    id: 4,
    name: "Sparkle Cleaning",
    service: "home cleaning",
    rating: 4.7,
    location: "Eastside",
    distance: "1.5 miles",
    price: "$23/hour"
  }
];

const ServiceSearch = () => {
  const [searchQuery, setSearchQuery] = useState('');
  const [location, setLocation] = useState('');
  const [results, setResults] = useState([]);
  const [isSearching, setIsSearching] = useState(false);

  const handleSearch = () => {
    setIsSearching(true);
    // Simulate API call with setTimeout
    setTimeout(() => {
      const filteredResults = mockProviders.filter(provider => 
        provider.service.toLowerCase().includes(searchQuery.toLowerCase()) &&
        (!location || provider.location.toLowerCase().includes(location.toLowerCase()))
      );
      setResults(filteredResults);
      setIsSearching(false);
    }, 500);
  };

  return (
    <div className="max-w-4xl mx-auto p-6">
      <Card className="mb-8">
        <CardHeader>
          <CardTitle>Find Local Service Providers</CardTitle>
          <CardDescription>Search for service providers in your area</CardDescription>
        </CardHeader>
        <CardContent>
          <div className="flex flex-col md:flex-row gap-4">
            <div className="flex-1">
              <div className="relative">
                <Search className="absolute left-2 top-3 h-4 w-4 text-gray-500" />
                <Input
                  type="text"
                  placeholder="What service do you need? (e.g., dog walker, home cleaning)"
                  value={searchQuery}
                  onChange={(e) => setSearchQuery(e.target.value)}
                  className="pl-8"
                />
              </div>
            </div>
            <div className="flex-1">
              <div className="relative">
                <MapPin className="absolute left-2 top-3 h-4 w-4 text-gray-500" />
                <Input
                  type="text"
                  placeholder="Location"
                  value={location}
                  onChange={(e) => setLocation(e.target.value)}
                  className="pl-8"
                />
              </div>
            </div>
            <Button 
              onClick={handleSearch}
              disabled={isSearching}
              className="md:w-24"
            >
              {isSearching ? 'Searching...' : 'Search'}
            </Button>
          </div>
        </CardContent>
      </Card>

      {results.length > 0 && (
        <div className="grid gap-4">
          {results.map(provider => (
            <Card key={provider.id}>
              <CardContent className="pt-6">
                <div className="flex justify-between items-start">
                  <div>
                    <h3 className="text-lg font-semibold">{provider.name}</h3>
                    <p className="text-sm text-gray-500">Service: {provider.service}</p>
                    <p className="text-sm text-gray-500">Location: {provider.location} ({provider.distance})</p>
                    <p className="text-sm text-gray-500">Price: {provider.price}</p>
                  </div>
                  <div className="text-right">
                    <div className="bg-green-100 text-green-800 px-2 py-1 rounded-full text-sm">
                      ★ {provider.rating}
                    </div>
                  </div>
                </div>
              </CardContent>
            </Card>
          ))}
        </div>
      )}

      {searchQuery && results.length === 0 && !isSearching && (
        <Card>
          <CardContent className="py-8">
            <p className="text-center text-gray-500">
              No service providers found matching your criteria. Try adjusting your search.
            </p>
          </CardContent>
        </Card>
      )}
