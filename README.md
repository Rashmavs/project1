# project1
//APP
/LOADING.TSX

export default function Loading() {
  return null
}

/PAGE.TSX

import { Search } from "lucide-react"
import DoctorListing from "@/components/doctor-listing"

export default function Home() {
  return (
    <div className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold text-center mb-8">Find Your Doctor</h1>
      <div className="relative mb-8 max-w-md mx-auto">
        <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" />
        <input
          type="text"
          placeholder="Search by name, specialty or location"
          className="w-full pl-10 pr-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>
      <DoctorListing />
    </div>
  )
}

//COMPONENTS
/doctor-listing.tsx
"use client"

import { useState } from "react"
import { Card, CardContent } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import { Star, MapPin, Calendar, Phone } from "lucide-react"
import { doctors } from "@/data/doctors"

export default function DoctorListing() {
  const [selectedSpecialty, setSelectedSpecialty] = useState<string | null>(null)

  // Get unique specialties for filter
  const specialties = [...new Set(doctors.map((doctor) => doctor.specialty))]

  // Filter doctors by specialty if one is selected
  const filteredDoctors = selectedSpecialty
    ? doctors.filter((doctor) => doctor.specialty === selectedSpecialty)
    : doctors

  return (
    <div>
      <div className="flex flex-wrap gap-2 mb-6">
        <Badge
          variant={selectedSpecialty === null ? "default" : "outline"}
          className="cursor-pointer"
          onClick={() => setSelectedSpecialty(null)}
        >
          All Specialties
        </Badge>
        {specialties.map((specialty) => (
          <Badge
            key={specialty}
            variant={selectedSpecialty === specialty ? "default" : "outline"}
            className="cursor-pointer"
            onClick={() => setSelectedSpecialty(specialty)}
          >
            {specialty}
          </Badge>
        ))}
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {filteredDoctors.map((doctor) => (
          <Card key={doctor.id} className="overflow-hidden hover:shadow-lg transition-shadow">
            <CardContent className="p-0">
              <div className="p-4">
                <div className="flex items-start gap-4">
                  <img
                    src={doctor.image || "/placeholder.svg"}
                    alt={doctor.name}
                    className="w-20 h-20 rounded-full object-cover border-2 border-gray-100"
                  />
                  <div>
                    <h3 className="font-bold text-lg">{doctor.name}</h3>
                    <p className="text-gray-500">{doctor.specialty}</p>
                    <div className="flex items-center mt-1">
                      {Array.from({ length: 5 }).map((_, i) => (
                        <Star
                          key={i}
                          className={`w-4 h-4 ${
                            i < doctor.rating ? "text-yellow-400 fill-yellow-400" : "text-gray-300"
                          }`}
                        />
                      ))}
                      <span className="ml-1 text-sm text-gray-600">({doctor.reviewCount})</span>
                    </div>
                  </div>
                </div>

                <div className="mt-4 space-y-2">
                  <div className="flex items-center text-sm">
                    <MapPin className="w-4 h-4 mr-2 text-gray-500" />
                    <span>{doctor.location}</span>
                  </div>
                  <div className="flex items-center text-sm">
                    <Calendar className="w-4 h-4 mr-2 text-gray-500" />
                    <span>{doctor.availability}</span>
                  </div>
                  <div className="flex items-center text-sm">
                    <Phone className="w-4 h-4 mr-2 text-gray-500" />
                    <span>{doctor.phone}</span>
                  </div>
                </div>

                <div className="mt-4 flex justify-between items-center">
                  <Badge variant="outline">{doctor.insurance ? "Insurance Accepted" : "Self Pay"}</Badge>
                  <Button size="sm">Book Appointment</Button>
                </div>
              </div>
            </CardContent>
          </Card>
        ))}
      </div>

      {filteredDoctors.length === 0 && (
        <div className="text-center py-8">
          <p className="text-gray-500">No doctors found for this specialty.</p>
        </div>
      )}
    </div>
  )
}

//DATA
/doctors.ts
export interface Doctor {
  id: number
  name: string
  specialty: string
  rating: number
  reviewCount: number
  location: string
  availability: string
  phone: string
  insurance: boolean
  image: string
}

export const doctors: Doctor[] = [
  {
    id: 1,
    name: "Dr. Sarah Johnson",
    specialty: "Cardiology",
    rating: 4.8,
    reviewCount: 124,
    location: "123 Medical Center, New York, NY",
    availability: "Mon, Wed, Fri: 9AM - 5PM",
    phone: "(212) 555-1234",
    insurance: true,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 2,
    name: "Dr. Michael Chen",
    specialty: "Dermatology",
    rating: 4.7,
    reviewCount: 98,
    location: "456 Health Plaza, New York, NY",
    availability: "Tue, Thu: 10AM - 6PM",
    phone: "(212) 555-5678",
    insurance: true,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 3,
    name: "Dr. Emily Rodriguez",
    specialty: "Pediatrics",
    rating: 4.9,
    reviewCount: 156,
    location: "789 Children's Center, New York, NY",
    availability: "Mon-Fri: 8AM - 4PM",
    phone: "(212) 555-9012",
    insurance: true,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 4,
    name: "Dr. James Wilson",
    specialty: "Orthopedics",
    rating: 4.6,
    reviewCount: 87,
    location: "321 Bone & Joint Clinic, New York, NY",
    availability: "Mon, Wed, Fri: 8AM - 6PM",
    phone: "(212) 555-3456",
    insurance: true,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 5,
    name: "Dr. Lisa Patel",
    specialty: "Neurology",
    rating: 4.8,
    reviewCount: 112,
    location: "654 Brain Health Center, New York, NY",
    availability: "Tue, Thu: 9AM - 5PM",
    phone: "(212) 555-7890",
    insurance: false,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 6,
    name: "Dr. Robert Kim",
    specialty: "Ophthalmology",
    rating: 4.7,
    reviewCount: 76,
    location: "987 Vision Care, New York, NY",
    availability: "Mon-Fri: 9AM - 4PM",
    phone: "(212) 555-2345",
    insurance: true,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 7,
    name: "Dr. Jennifer Taylor",
    specialty: "Psychiatry",
    rating: 4.9,
    reviewCount: 134,
    location: "246 Mental Health Clinic, New York, NY",
    availability: "Mon, Wed: 10AM - 7PM",
    phone: "(212) 555-6789",
    insurance: true,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 8,
    name: "Dr. David Martinez",
    specialty: "Cardiology",
    rating: 4.6,
    reviewCount: 91,
    location: "135 Heart Center, New York, NY",
    availability: "Tue, Thu, Fri: 8AM - 5PM",
    phone: "(212) 555-0123",
    insurance: true,
    image: "/placeholder.svg?height=80&width=80",
  },
  {
    id: 9,
    name: "Dr. Sophia Lee",
    specialty: "Dermatology",
    rating: 4.8,
    reviewCount: 108,
    location: "579 Skin Care Clinic, New York, NY",
    availability: "Mon, Wed, Fri: 9AM - 6PM",
    phone: "(212) 555-4567",
    insurance: false,
    image: "/placeholder.svg?height=80&width=80",
  },
]






