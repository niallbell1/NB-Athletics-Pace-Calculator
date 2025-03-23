import React, { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Select, SelectItem } from "@/components/ui/select";
import Image from "next/image";
import { FaFacebook, FaInstagram } from "react-icons/fa";

const raceDistances = ["5K", "10K", "Half Marathon", "Marathon"];

const PaceCalculator = () => {
  const [time, setTime] = useState("");
  const [distance, setDistance] = useState("");
  const [pace, setPace] = useState("");
  const [gender, setGender] = useState("Male");
  const [selectedRace, setSelectedRace] = useState("5K");
  const [name, setName] = useState("");
  const [savedProfiles, setSavedProfiles] = useState([]);

  useEffect(() => {
    const storedProfiles = JSON.parse(localStorage.getItem("runnerProfiles")) || [];
    setSavedProfiles(storedProfiles);
  }, []);

  const calculatePace = () => {
    if (time && distance) {
      const totalMinutes = parseFloat(time);
      const totalDistance = parseFloat(distance);
      const pacePerMile = (totalMinutes / totalDistance).toFixed(2);
      setPace(pacePerMile);
    }
  };

  const saveProfile = () => {
    if (name && pace) {
      const newProfile = { name, time, distance, pace, gender, selectedRace };
      const updatedProfiles = [...savedProfiles, newProfile];
      setSavedProfiles(updatedProfiles);
      localStorage.setItem("runnerProfiles", JSON.stringify(updatedProfiles));
    }
  };

  return (
    <div className="p-6 max-w-lg mx-auto text-center">
      <Image src="/mnt/data/image.png" alt="NB Athletics Coach Logo" width={150} height={150} className="mx-auto mb-4" />
      <h1 className="text-2xl font-bold mb-4">NB Athletics Pace Calculator</h1>
      <Card>
        <CardContent>
          <Input placeholder="Name" value={name} onChange={(e) => setName(e.target.value)} className="mb-2" />
          <Input placeholder="Time (minutes)" value={time} onChange={(e) => setTime(e.target.value)} className="mb-2" />
          <Input placeholder="Distance (miles)" value={distance} onChange={(e) => setDistance(e.target.value)} className="mb-2" />
          <Select value={gender} onChange={(e) => setGender(e.target.value)} className="mb-2">
            <SelectItem value="Male">Male</SelectItem>
            <SelectItem value="Female">Female</SelectItem>
          </Select>
          <Select value={selectedRace} onChange={(e) => setSelectedRace(e.target.value)} className="mb-2">
            {raceDistances.map((race) => (
              <SelectItem key={race} value={race}>{race}</SelectItem>
            ))}
          </Select>
          <Button onClick={calculatePace} className="w-full mb-2">Calculate Pace</Button>
          {pace && <p className="text-lg">Pace: {pace} min/mile</p>}
          <Button onClick={saveProfile} className="w-full mt-2">Save Profile</Button>
        </CardContent>
      </Card>
      <div className="flex justify-center gap-4 mt-4">
        <a href="https://www.facebook.com/profile.php?id=61571612726559" target="_blank" rel="noopener noreferrer" className="text-blue-600 text-2xl">
          <FaFacebook />
        </a>
        <a href="https://www.instagram.com/nbathleticscoach/" target="_blank" rel="noopener noreferrer" className="text-pink-500 text-2xl">
          <FaInstagram />
        </a>
      </div>
    </div>
  );
};

export default PaceCalculator;
