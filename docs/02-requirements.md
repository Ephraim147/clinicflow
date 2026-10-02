# Requirements: ClinicFlow

## 1. MVP Scope

### In scope (Version 1)
1. Users can log in (roles: admin, receptionist, doctor)
2. Admin can add and manage doctors
3. Doctors have weekly working hours
4. Receptionist can add and search patients
5. Receptionist can book, cancel, and reschedule appointments
6. System blocks double-booking
7. Receptionists and doctors can see a daily schedule

### Out of scope (Future)
- SMS/email reminders
- No-show prediction (ML)
- Patient self-booking portal
- Waitlist auto-fill
- Multi-clinic support
- Storing medical records or diagnoses (never planned: privacy risk)

## 2. Functional Requirements
- FR-1: A user can log in with an email and password.
- FR-2: The system must not allow two active appointments for the same doctor at overlapping times.
- FR-3: An admin can add a doctor with name and specialization.
- FR-4: An admin can set a doctor's working hours for each weekday.
- FR-5: A receptionist can add a patient with name and phone number.
- FR-6: A receptionist can search patients by name or phone number.
- FR-7: A receptionist can book an appointment for a patient with a doctor, date, and time.
- FR-8: The system must only allow bookings inside the doctor's working hours.
- FR-9: A receptionist can cancel an appointment, and the slot becomes free again.
- FR-10: A receptionist can reschedule an appointment to a new free slot.
- FR-11: A receptionist can view the daily schedule of any doctor.
- FR-12: A doctor can view only their own daily schedule.
- FR-13: Each appointment has a status: booked, cancelled, completed, or no-show.
- FR-14: The system records who created or changed each appointment, and when.

## 3. Non-Functional Requirements
- NFR-1 (Security): Passwords must never be stored as plain text.
- NFR-2 (Security): Only logged-in users can access any appointment or patient data.
- NFR-3 (Security): Each role can only perform actions allowed for that role.
- NFR-4 (Performance): Booking an appointment should respond in under 2 seconds.
- NFR-5 (Reliability): Two users booking the same slot at the same moment must never both succeed.
- NFR-6 (Privacy): Only synthetic (fake) patient data is used in development, testing, and demos.
- NFR-7 (Privacy): The system stores only the minimum patient data needed (name, phone). No diagnoses.
- NFR-8 (Usability): A receptionist should be able to book an appointment in 5 steps or fewer.
- NFR-9 (Maintainability): Errors are written to a log with a timestamp.
- NFR-10 (Quality): Core booking rules are covered by automated tests.

## 4. User Stories
- US-1: As a receptionist, I want to book an appointment for a patient, so that the doctor's slot is reserved.
- US-2: As a receptionist, I want the system to refuse a double-booking, so that two patients never arrive for the same slot.
- US-3: As a receptionist, I want to cancel an appointment, so that the slot can be given to someone else.
- US-4: As a receptionist, I want to reschedule an appointment, so that I don't have to cancel and re-enter everything.
- US-5: As a receptionist, I want to search for a patient by phone number, so that I can find them quickly