import React, { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { FaFacebook, FaInstagram } from "react-icons/fa";

const raceDistances = [
  "400m", "800m", "1km", "1500m", "1 mile", "2km", "3km", 
  "5K", "10K", "5 mile", "10 mile", "Half Marathon", "Marathon"
];

const PaceCalculator = () => {
  const [time, setTime] = useState("");
  const [distance, setDistance] = useState("");
  const [pace, setPace] = useState("");
  const [gender, setGender] = useState("Male");
  const [selectedRace, setSelectedRace] = useState("5K");
  const [name, setName] = useState("");
  const [savedProfiles, setSavedProfiles] = useState([]);

  useEffect(() => {
    if (typeof window !== "undefined") {
      const storedProfiles = JSON.parse(localStorage.getItem("runnerProfiles")) || [];
      setSavedProfiles(storedProfiles);
    }
  }, []);

  const calculatePace = () => {
    if (!time || !distance || isNaN(time) || isNaN(distance)) {
      alert("Please enter valid numbers for time and distance.");
      return;
    }
    const totalMinutes = parseFloat(time);
    const totalDistance = parseFloat(distance);
    const pacePerMile = (totalMinutes / totalDistance).toFixed(2);
    setPace(pacePerMile);
  };

  const saveProfile = () => {
    if (!name || !pace) {
      alert("Please enter a name and calculate pace before saving.");
      return;
    }
    const newProfile = { name, time, distance, pace, gender, selectedRace };
    const updatedProfiles = [...savedProfiles, newProfile];
    setSavedProfiles(updatedProfiles);
    localStorage.setItem("runnerProfiles", JSON.stringify(updatedProfiles));
  };

  return (
    <div className="p-6 max-w-lg mx-auto text-center">
      <h1 className="text-2xl font-bold mb-4">NB Athletics Pace Calculator</h1>
      <Card>
        <CardContent>
          <Input
            placeholder="Name"
            value={name}
            onChange={(e) => setName(e.target.value)}
            className="mb-2"
            aria-label="Name"
          />
          <Input
            placeholder="Time (minutes)"
            value={time}
            onChange={(e) => setTime(e.target.value)}
            className="mb-2"
            aria-label="Time"
          />
          <Input
            placeholder="Distance (miles)"
            value={distance}
            onChange={(e) => setDistance(e.target.value)}
            className="mb-2"
            aria-label="Distance"
          />
          
          <select
            value={gender}
            onChange={(e) => setGender(e.target.value)}
            className="mb-2 p-2 border rounded w-full"
            aria-label="Gender"
          >
            <option value="Male">Male</option>
            <option value="Female">Female</option>
          </select>
          
          <select
            value={selectedRace}
            onChange={(e) => setSelectedRace(e.target.value)}
            className="mb-2 p-2 border rounded w-full"
            aria-label="Race Distance"
          >
            {raceDistances.map((race) => (
              <option key={race} value={race}>{race}</option>
            ))}
          </select>
          
          <Button onClick={calculatePace} className="w-full mb-2">Calculate Pace</Button>
          {pace && <p className="text-lg">Pace: {pace} min/mile</p>}
          <Button onClick={saveProfile} className="w-full mt-2">Save Profile</Button>
        </CardContent>
      </Card>
      
      <div className="flex justify-center gap-4 mt-4">
        <a
          href="https://www.facebook.com/profile.php?id=61571612726559"
          target="_blank"
          rel="noopener noreferrer"
          className="text-blue-600 text-2xl"
          aria-label="Facebook"
        >
          <FaFacebook />
        </a>
        <a
          href="https://www.instagram.com/nbathleticscoach/"
          target="_blank"
          rel="noopener noreferrer"
          className="text-pink-500 text-2xl"
          aria-label="Instagram"
        >
          <FaInstagram />
        </a>
      </div>
    </div>
  );
};

export default PaceCalculator;
