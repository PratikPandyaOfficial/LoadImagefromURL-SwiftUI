# LoadImagefromURL-SwiftUI
Load image from URL with Success, Fail and Empty stats.

import SwiftUI

struct ContentView: View {
    
    private let imageURL: String = "https://img.freepik.com/free-vector/colorful-letter-gradient-logo-design_474888-2309.jpg?w=740&t=st=1690884500~exp=1690885100~hmac=c38b85b9027475a8471a9b16cc9c2853d5b1c9e6229426574b616c150607e259"
    
    var body: some View {
//MARK: - 1. Basic
        //AsyncImage(url: URL(string: imageURL))
        
//MARK: - 2. Scale
        //AsyncImage(url: URL(string: imageURL), scale: 2.0)
        
//MARK: - 3. PlaceHolder
        /*
        AsyncImage(url: URL(string: imageURL)){ img in
            img
                .imageModifier()
        } placeholder: {
            Image(systemName: "photo.circle.fill")
                .iconModifier()
        }
        .padding(40)
         */
        
//MARK: - 4. Phase
        /*
         AsyncImage(url: URL(string: imageURL)){ phase in
            if let image = phase.image{
                image.imageModifier()
            }
            else if phase.error != nil{
                Image(systemName: "ant.circle.fill")
                    .iconModifier()
            }
            else{
                Image(systemName: "photo.circle.fill")
                    .iconModifier()
            }
        }
        .padding(40)
         */
        
//MARK: - 5. Animation
        AsyncImage(url: URL(string: imageURL), transaction: Transaction(animation: .spring(response: 0.5, dampingFraction: 0.6, blendDuration: 0.25))){ phase in
            switch phase{
            case .success(let img):
                img
                    .imageModifier()
                    //.transition(.move(edge: .bottom))
                    //.transition(.slide)
                    .transition(.scale)
                    //.transition(.identity)
                    //.transition(.opacity)
            case .failure(_):
                Image(systemName: "ant.circle.fill")
                    .iconModifier()
            case .empty:
                Image(systemName: "photo.circle.fill")
                    .iconModifier()
            @unknown default:
                ProgressView()
            }
        }
        .padding(40)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

//MARK: - image extension
extension Image{
    func imageModifier() -> some View{
        self
            .resizable()
            .scaledToFit()
    }
    func iconModifier() -> some View{
        self
            .imageModifier()
            .frame(maxWidth: 125)
            .foregroundColor(.purple)
            .opacity(0.5)
    }
}
