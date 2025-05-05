# gestione-assenze

import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { CalendarDays, User, CheckCircle } from "lucide-react";
import { format } from "date-fns";

const players = [
  "Luca Rossi",
  "Marco Bianchi",
  "Giovanni Verdi",
  "Andrea Neri",
  "Stefano Gialli",
];

const trainingDates = generateDates(new Date("2025-07-01"), new Date("2026-06-30"));

function generateDates(start, end) {
  const dates = [];
  let current = new Date(start);
  while (current <= end) {
    if ([2, 3, 5].includes(current.getDay())) { // Martedì (2), Mercoledì (3), Venerdì (5)
      dates.push(new Date(current));
    }
    current.setDate(current.getDate() + 1);
  }
  return dates;
}

function groupDatesByMonth(dates) {
  return dates.reduce((acc, date) => {
    const key = format(date, "MMMM yyyy");
    if (!acc[key]) acc[key] = [];
    acc[key].push(date);
    return acc;
  }, {});
}

export default function Dashboard() {
  const [assenze, setAssenze] = useState([]);
  const [selectedDate, setSelectedDate] = useState("");
  const [selectedPlayer, setSelectedPlayer] = useState(players[0]);

  const calendarByMonth = groupDatesByMonth(trainingDates);

  const handleRegisterAbsence = () => {
    if (selectedDate && selectedPlayer) {
      setAssenze([...assenze, { player: selectedPlayer, date: selectedDate }]);
      alert(`Assenza registrata per ${selectedPlayer} il ${selectedDate}`);
    }
  };

  return (
    <main className="min-h-screen bg-white text-red-700 p-6">
      <h1 className="text-3xl font-bold mb-6 text-center">Dashboard Assenze Squadra</h1>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        <Card className="bg-red-100">
          <CardContent className="p-4 flex items-center gap-4">
            <User className="w-10 h-10 text-red-700" />
            <div>
              <h2 className="text-xl font-semibold">Giocatori</h2>
              <p className="text-sm">{players.length} registrati</p>
            </div>
          </CardContent>
        </Card>

        <Card className="bg-red-100">
          <CardContent className="p-4 flex items-center gap-4">
            <CalendarDays className="w-10 h-10 text-red-700" />
            <div>
              <h2 className="text-xl font-semibold">Prossimo Allenamento</h2>
              <p className="text-sm">{format(trainingDates[0], 'EEEE d MMMM yyyy')}</p>
            </div>
          </CardContent>
        </Card>

        <Card className="bg-red-100">
          <CardContent className="p-4 flex items-center gap-4">
            <CheckCircle className="w-10 h-10 text-red-700" />
            <div>
              <h2 className="text-xl font-semibold">Presenze Oggi</h2>
              <p className="text-sm">18 presenti</p>
            </div>
          </CardContent>
        </Card>
      </div>

      <div className="mt-8">
        <h2 className="text-2xl font-bold mb-4">Rosa Giocatori</h2>
        <ul className="list-disc pl-6">
          {players.map((player, index) => (
            <li key={index} className="text-base mb-1">{player}</li>
          ))}
        </ul>
      </div>

      <div className="mt-8">
        <h2 className="text-2xl font-bold mb-4">Calendario Allenamenti</h2>
        {Object.entries(calendarByMonth).map(([month, dates], index) => (
          <div key={index} className="mb-6">
            <h3 className="text-xl font-semibold mb-2">{month}</h3>
            <ul className="grid grid-cols-2 md:grid-cols-4 gap-2 text-sm">
              {dates.map((date, i) => (
                <li key={i} className="bg-red-100 p-2 rounded shadow-sm">
                  {format(date, 'dd/MM/yyyy')}
                </li>
              ))}
            </ul>
          </div>
        ))}
      </div>

      <div className="mt-8 max-w-xl mx-auto p-4 bg-red-50 rounded">
        <h2 className="text-xl font-semibold mb-4">Registra Nuova Assenza</h2>
        <div className="flex flex-col gap-4">
          <select
            value={selectedPlayer}
            onChange={(e) => setSelectedPlayer(e.target.value)}
            className="border p-2 rounded"
          >
            {players.map((player, idx) => (
              <option key={idx} value={player}>{player}</option>
            ))}
          </select>
          <input
            type="date"
            value={selectedDate}
            onChange={(e) => setSelectedDate(e.target.value)}
            className="border p-2 rounded"
          />
          <Button onClick={handleRegisterAbsence} className="bg-red-600 text-white hover:bg-red-700">
            Registra Assenza
          </Button>
        </div>
      </div>
    </main>
  );
}
