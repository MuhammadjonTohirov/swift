# Amaliy misollar

<details>

<summary>Misol 1: Rasmlar galerayasi dasturi</summary>

```swift
class ImageGalleryViewController: UIViewController {
    @IBOutlet weak var collectionView: UICollectionView!
    private var images: [UIImage] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
        loadThumbnails()
    }
    
    func loadThumbnails() {
        // Asosiy UI elementlarni darhol ko'rsatish - userInteractive
        DispatchQueue.global(qos: .userInteractive).async {
            let placeholders = self.generatePlaceholders()
            
            DispatchQueue.main.async {
                self.images = placeholders
                self.collectionView.reloadData()
            }
        }
        
        // Miniatyuralarni yuklash - userInitiated
        DispatchQueue.global(qos: .userInitiated).async {
            let thumbnails = self.loadThumbnailsFromDisk()
            
            DispatchQueue.main.async {
                self.images = thumbnails
                self.collectionView.reloadData()
            }
        }
        
        // To'liq o'lchamdagi rasmlarni oldindan yuklash - utility
        DispatchQueue.global(qos: .utility).async {
            _ = self.preloadFullSizeImages()
        }
        
        // Metadatani yangilash, eskirgan rasmlarni o'chirish - background
        DispatchQueue.global(qos: .background).async {
            self.updateMetadata()
            self.cleanupOldImages()
        }
    }
    
    // Boshqa metodlar...
}
```

</details>

<details>

<summary>Misol 2: Dokument tahrirlash dasturi</summary>

```swift
class DocumentEditorViewModel {
    private let documentQueue = DispatchQueue(label: "com.example.documentQueue", qos: .userInitiated)
    private let autoSaveQueue = DispatchQueue(label: "com.example.autoSaveQueue", qos: .utility)
    private let indexingQueue = DispatchQueue(label: "com.example.indexingQueue", qos: .background)
    
    var document: Document?
    
    func openDocument(id: String, completion: @escaping (Document?) -> Void) {
        // Hujjatni ochish - foydalanuvchi kutayotgan operatsiya
        documentQueue.async {
            let doc = self.loadDocumentFromStorage(id: id)
            
            DispatchQueue.main.async {
                self.document = doc
                completion(doc)
            }
            
            // Hujjat ochilgandan so'ng, qidiruv indekslarini yangilash - orqa fon vazifasi
            self.indexingQueue.async {
                self.updateSearchIndexes(for: doc)
            }
        }
    }
    
    func saveChanges() {
        guard let doc = document else { return }
        
        // Hujjatni saqlash - foydalanuvchi tomonidan boshlangan
        documentQueue.async {
            self.saveDocumentToStorage(doc)
        }
    }
    
    func setupAutoSave() {
        // Avtomatik saqlash - o'rtacha ustuvorlik
        let autoSaveWorkItem = DispatchWorkItem(qos: .utility) {
            guard let doc = self.document else { return }
            self.saveDocumentToStorage(doc)
        }
        
        // Har 5 daqiqada avtomatik saqlash
        autoSaveQueue.asyncAfter(deadline: .now() + 300, execute: autoSaveWorkItem)
    }
    
    // Boshqa metodlar...
}
```

</details>
