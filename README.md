// Noemi Exchange Phase 1: Safe Conversion & Display Script

const LAMPORTS_PER_SOL = 1_000_000_000;

export function convertValue(amount, chain) {
  if (chain.toLowerCase() === "eth") {
    return {
      raw: BigInt(amount * 1e18),
      unit: "wei"
    };
  }
  if (chain.toLowerCase() === "sol") {
    return {
      raw: Math.floor(amount * LAMPORTS_PER_SOL),
      unit: "lamports"
    };
  }
  throw new Error("Unsupported chain");
}

// Example usage
console.log("ETH 0.1 →", convertValue(0.1, "eth"));
console.log("SOL 1.5 →", convertValue(1.5, "sol"));

// Dashboard data fetch (Firestore)
import { getFirestore, doc, getDoc } from "@firebase/firestore";
const db = getFirestore();

async function fetchPlatformCap() {
  const docRef = doc(db, "platform_cap", "current");
  const snapshot = await getDoc(docRef);
  if (snapshot.exists()) {
    return snapshot.data();
  }
  return null;
}

// Display in Fireball app
fetchPlatformCap().then(data => {
  console.log("Noemi Platform Cap:", data);
});
