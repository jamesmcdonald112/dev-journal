```ts
export const TEST_PRODUCT = {
  _id: "674b9f51e223f0b928b11111",
  title: "iPhone 15 Pro – Titanium Blue",
  slug: "iphone-15-pro-titanium-blue",
  shortDescription: "A lightweight, durable smartphone with exceptional camera performance.",
  longDescription: `
The iPhone 15 Pro features a titanium frame, upgraded thermal performance,
and Apple's A17 Pro chipset designed for extreme efficiency and power.
The 48 MP main camera supports improved low-light capture and 3D spatial video.
The device includes USB-C, Wi-Fi 6E, and ProMotion 120Hz support.

Users choose the iPhone 15 Pro for its performance, camera quality, battery optimization,
and ecosystem integration. Ideal for photography enthusiasts, mobile creators,
and anyone needing a professional-grade smartphone experience.
  `,
  specs: {
    display: "6.1\" LTPO OLED, 120Hz ProMotion",
    chipset: "Apple A17 Pro",
    storageOptions: "128 GB / 256 GB / 512 GB / 1 TB",
    camera: "48 MP main, 12 MP ultra-wide, 12 MP telephoto",
    battery: "3,274 mAh",
    connectivity: "5G, Wi-Fi 6E, Bluetooth 5.3, USB-C",
    waterResistance: "IP68",
    charging: "20W wired, 15W MagSafe wireless"
  },
  reviews: [
    "Amazing camera quality and extremely smooth performance.",
    "Battery life is solid and the new titanium frame feels premium.",
    "A big upgrade if you care about photography and video capture."
  ],
  price: 1199,
  images: [
    "https://example.com/iphone15pro-front.jpg",
    "https://example.com/iphone15pro-back.jpg"
  ],
  createdAt: new Date(),
  updatedAt: new Date(),
};
```