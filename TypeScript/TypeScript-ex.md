## TypeScript 예제

- Dictionary

```TypeScript
type Word = {
    term: string
    def: string
}
type Words = { //Dict 클래스에 들어갈 단어 객체의 타입
    [key: string]: string //인덱스 서명: Words는 string을 속성으로 가지는 객체
}
class Dict {
    private words: Words //임의로 변경되어서는 안 되므로 외부 접근 차단
    constructor () {
        this.words = {} //클래스 내부에서 words 객체에 접근 가능하도록 만듦
    }
    add(term: string, def: string) {
        if (this.get(term)) {
            this.words[term] = def
        }
    }
    get(term: string) {
        return this.words[term]
    }
    delete(term: string) {
        delete this.words[term]
    }
    update(term: string, newDef: string) {
        if (this.get(term)) {
            this.words[term] = newDef
        }
    }
    showAll() {
        let output = "\n--- Dictionary content ---\n"
        Object.keys(this.words).forEach((term) =>
            output += `${term} : ${this.words[term]}\n`
        )
        output += "--- End of Dictionary ---\n"
        console.log(output)
    }
    count() {
        return Object.keys(this.words).length //Object.keys()는 객체의 모든 key의 값들을 배열 형태로 가져옴
    }
    upsert(term: string, def: string) {
        if (this.get(term)) {
            this.update(term, def)
        } else {
            this.add(term, def)
        }
    }
    exists(term: string) {
        return this.get(term) ? true : false
    }
    bulkAdd(words: Word[]) {
        words.forEach(word => this.add(word.term, word.def))
    }
    bulkDelete(terms: string[]) {
        terms.forEach(term => this.delete(term))
    }
}

const dict = new Dict()
```

- LocalStorage API

```TypeScript
interface Items<T> {
    [key: string]: T
}

abstract class LocalStorage<T> {
    protected items: Items<T>
    constructor () {
        this.items = {}
    }
    abstract length(): number
    abstract key(index: number): T
    abstract getItem(key: string): T
    abstract setItem(key: string, value: T): void
    abstract removeItem(key: string): void
    abstract clear(): void
}

class MyStorage extends LocalStorage<string> {
    constructor () {

    }
    public length() {
        return Object.keys(this.items).length
    }
    public key(index: number) {
        return Object.keys(this.items)[index]
    }
    public getItem(key: string) {
        return this.items[key]
    }
    public setItem(key: string, value: string) {
        this.items[key] = value
    }
    public removeItem(key: string) {
        delete this.items[key]
    }
    clear() {
        this.items = {}
    }
}
```

- Geolocation API

```TypeScript
interface GeolocationCoords {
    latitude: number
    longitude: number
    altitude: number
    accuracy: number
    altitudeAccuracy: number
    heading: number
    speed: number
}
type Position = {
    coords: GeolocationCoords
}
type GeoError = {
    code: number
    message: string
}
type SuccessFunction = (position: Position) => void
type ErrorFunction = (error: GeoError) => void
type GeoOptions = {
    maximumAge: number
    timeOut: number
    enableHighAccuracy: number
}

type GetCurrentPosition = {
    (success: SuccessFunction): void
    (success: SuccessFunction, error: ErrorFunction): void
    (success: SuccessFunction, error: ErrorFunction, options: GeoOptions): void
}

type WatchCurrentPosition = {
    (success: SuccessFunction): number
    (success: SuccessFunction, error: ErrorFunction): number
    (success: SuccessFunction, error: ErrorFunction, options: GeoOptions): number
}

interface GeolocationAPI {
    getCurrentPosition: GetCurrentPosition
    watchCurrentPosition: WatchCurrentPosition
    clearWatch: (id: number) => void
}

class Geolocator implements GeolocationAPI {
    getCurrentPosition: GetCurrentPosition = (
        success: SuccessFunction,
        error?: ErrorFunction,
        options?: GeoOptions
    ) => {
        return
    }
    watchCurrentPosition: WatchCurrentPosition = (
        success: SuccessFunction,
        error?: ErrorFunction,
        options?: GeoOptions
    ) => {
        return
    }
    clearWatch = (id: number) => {}
}
```
