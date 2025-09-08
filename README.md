import SwiftUI

// MARK: - Mock Models (replace with real API later)
struct AppUser: Identifiable {
    let id = UUID()
    let fullName: String
    let userType: String
    let department: String
}

struct AppRide: Identifiable {
    let id = UUID()
    let departureLocation: String
    let destination: String
    let departureTime: Date
    let rideType: String // "offering" or "requesting"
}

// MARK: - Dashboard View
struct AppDashboardView: View {
    @State private var user: AppUser? = AppUser(
        fullName: "Piyush Bartwal",
        userType: "Student",
        department: "Computer Science"
    )
    
    @State private var recentRides: [AppRide] = [
        AppRide(departureLocation: "Campus Gate", destination: "City Center", departureTime: Date(), rideType: "offering"),
        AppRide(departureLocation: "Library", destination: "Hostel", departureTime: Date().addingTimeInterval(3600), rideType: "requesting")
    ]
    
    @State private var isLoading: Bool = false

    var body: some View {
        NavigationView {
            ZStack {
                LinearGradient(
                    gradient: Gradient(colors: [.blue.opacity(0.1), .white, .green.opacity(0.1)]),
                    startPoint: .topLeading,
                    endPoint: .bottomTrailing
                )
                .edgesIgnoringSafeArea(.all)

                if isLoading {
                    ProgressView("Loading...")
                        .progressViewStyle(CircularProgressViewStyle())
                        .scaleEffect(1.5)
                } else {
                    ScrollView {
                        VStack(spacing: 24) {

                            // MARK: Welcome Header
                            VStack(spacing: 12) {
                                ZStack {
                                    RoundedRectangle(cornerRadius: 20)
                                        .fill(LinearGradient(
                                            gradient: Gradient(colors: [.blue, .green]),
                                            startPoint: .leading,
                                            endPoint: .trailing
                                        ))
                                        .frame(width: 70, height: 70)

                                    Image(systemName: "bolt.fill")
                                        .foregroundColor(.white)
                                        .font(.system(size: 30))
                                }

                                Text("Welcome to Campus Connect")
                                    .font(.largeTitle)
                                    .fontWeight(.bold)

                                if let user = user {
                                    Text("\(user.fullName) • \(user.userType) • \(user.department)")
                                        .font(.subheadline)
                                        .foregroundColor(.gray)
                                }
                            }

                            // MARK: Quick Actions
                            VStack(spacing: 16) {
                                AppQuickActionCard(
                                    title: "Ask Campus Chatbot",
                                    description: "Get instant answers about campus life, academics, and policies",
                                    systemIcon: "message.fill",
                                    stat: "24/7 Available"
                                )
                                AppQuickActionCard(
                                    title: "Find or Offer Rides",
                                    description: "Safe carpooling with verified MUJ community members",
                                    systemIcon: "car.fill",
                                    stat: "\(recentRides.count) Active Rides"
                                )
                            }

                            // MARK: Recent Rides
                            if !recentRides.isEmpty {
                                VStack(alignment: .leading, spacing: 12) {
                                    Text("Recent Carpooling Activity")
                                        .font(.headline)

                                    ForEach(recentRides) { ride in
                                        HStack {
                                            Circle()
                                                .fill(LinearGradient(
                                                    gradient: Gradient(colors: [.green, .blue]),
                                                    startPoint: .leading,
                                                    endPoint: .trailing
                                                ))
                                                .frame(width: 40, height: 40)
                                                .overlay(
                                                    Image(systemName: "car.fill")
                                                        .foregroundColor(.white)
                                                )

                                            VStack(alignment: .leading) {
                                                Text("\(ride.departureLocation) → \(ride.destination)")
                                                    .font(.subheadline)
                                                    .fontWeight(.medium)
                                                Text(ride.departureTime, style: .time)
                                                    .font(.caption)
                                                    .foregroundColor(.gray)
                                            }

                                            Spacer()

                                            Text(ride.rideType.capitalized)
                                                .font(.caption)
                                                .padding(6)
                                                .background(Color.blue.opacity(0.1))
                                                .cornerRadius(8)
                                        }
                                        .padding()
                                        .background(Color.gray.opacity(0.1))
                                        .cornerRadius(12)
                                    }
                                }
                            }

                            // MARK: Features
                            VStack(spacing: 16) {
                                AppFeatureCard(
                                    icon: "shield.fill",
                                    title: "Verified Community",
                                    description: "College email verification ensures safe interactions"
                                )
                                AppFeatureCard(
                                    icon: "book.fill",
                                    title: "Comprehensive Knowledge",
                                    description: "Student & faculty resources in one place"
                                )
                                AppFeatureCard(
                                    icon: "person.3.fill",
                                    title: "Community-Driven",
                                    description: "Built by students, for the MUJ community"
                                )
                            }

                            // MARK: Call to Action
                            HStack {
                                Image(systemName: "sparkles")
                                    .foregroundColor(.blue)
                                Text("Making campus life easier, one connection at a time")
                                    .font(.footnote)
                                    .foregroundColor(.blue)
                            }
                            .padding(.top, 20)
                        }
                        .padding()
                    }
                }
            }
        }
    }
}

// MARK: - Quick Action Card
struct AppQuickActionCard: View {
    let title: String
    let description: String
    let systemIcon: String
    let stat: String

    var body: some View {
        VStack(alignment: .leading, spacing: 8) {
            HStack {
                Image(systemName: systemIcon)
                    .foregroundColor(.white)
                    .padding()
                    .background(LinearGradient(
                        gradient: Gradient(colors: [.blue, .green]),
                        startPoint: .leading,
                        endPoint: .trailing
                    ))
                    .clipShape(RoundedRectangle(cornerRadius: 12))

                VStack(alignment: .leading) {
                    Text(title)
                        .font(.headline)
                    Text(description)
                        .font(.subheadline)
                        .foregroundColor(.secondary)
                }

                Spacer()
            }

            Text(stat)
                .font(.caption)
                .foregroundColor(.blue)
                .padding(.top, 4)
        }
        .padding()
        .background(Color.white)
        .cornerRadius(12)
        .shadow(radius: 4)
    }
}

// MARK: - Feature Card
struct AppFeatureCard: View {
    let icon: String
    let title: String
    let description: String

    var body: some View {
        HStack(spacing: 16) {
            Image(systemName: icon)
                .foregroundColor(.blue)
                .frame(width: 40, height: 40)
                .background(Color.blue.opacity(0.1))
                .clipShape(RoundedRectangle(cornerRadius: 8))

            VStack(alignment: .leading) {
                Text(title)
                    .font(.headline)
                Text(description)
                    .font(.subheadline)
                    .foregroundColor(.secondary)
            }

            Spacer()
        }
        .padding()
        .background(Color.white)
        .cornerRadius(12)
        .shadow(radius: 4)
    }
}

// MARK: - Preview
struct AppDashboardView_Previews: PreviewProvider {
    static var previews: some View {
        AppDashboardView()
    }
}

