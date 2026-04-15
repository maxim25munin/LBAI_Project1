# Expected inferred facts checklist

Use this checklist after running the reasoner (HermiT or Pellet).

## Expected inferred facts

- [ ] `Apt1` is inferred as `LeasedApartment` (because `Alice rents Apt1` and `rents` is inverse of `is-rented-by`).
- [ ] `Alice` is inferred as `Tenant` (person living in a leased apartment).
- [ ] `Bob` is inferred as `Owner` (owns `Apt2`).
- [ ] `Charlie` is inferred as `Owner` (owns `Apt3`).
- [ ] `Dave` is inferred as `Owner` (owns `Apt1`).
- [ ] `Apt1`, `Apt2`, `Apt3`, and `Apt4` are inferred to be located in some `Neighborhood` (via transitive `located-in`: apartment -> building -> neighborhood).
- [ ] `Sonnenwendviertel` is inferred as `WellLocatedNeighborhood` (has metro `Hauptbahnhof`).
- [ ] `Mariahilf` is inferred as `WellLocatedNeighborhood` (has metro stations).
- [ ] `DonauCity` is inferred as `WellLocatedNeighborhood` (has metro stations).
- [ ] `Fasanviertel` is inferred as `WellLocatedNeighborhood` (near `Sonnenwendviertel`, and `is-near` is symmetric).
- [ ] `Mariahilf` is inferred as `UrbanNeighborhood` (at least 2 shops, at least 2 restaurants, well-located).
- [ ] `DonauCity` is inferred as `UrbanNeighborhood` (at least 2 shops, at least 2 restaurants, well-located).

## Things that may not be inferred

- [ ] `ResidentialNeighborhood` may not be inferred under the Open World Assumption, because proving "contains only non-Industrial" needs closure-style information.
- [ ] `FamilyNeighborhood` may fail if `contains` facts are not connected to `has-amenity` (unless you added a bridge axiom such as `has-amenity ⊑ located-in` and/or explicit `has-amenity` assertions).
- [ ] `Landlord` for `Charlie` is not expected unless `Apt3` is also known/inferred as leased.

## If missing inference, check this axiom (debug table)

| Missing inference | Check this axiom / modeling item | Why it matters |
|---|---|---|
| `Apt1 : LeasedApartment` | `LeasedApartment ≡ Apartment ⊓ (is-rented-by some Person)` and `rents ≡ inverse(is-rented-by)` | Without inverse alignment, rent facts do not imply leased-apartment classification. |
| `Alice : Tenant` | `Tenant ≡ Person ⊓ (lives-in some LeasedApartment)` and `Alice lives-in Apt1` | Tenant requires both personhood and living in a leased apartment. |
| `Bob/Charlie/Dave : Owner` | `Owner ≡ Person ⊓ (owns some Apartment)` and apartment typing for owned targets | If owned individual is not typed/inferred as `Apartment`, owner inference may not fire. |
| `Sonnenwendviertel : WellLocatedNeighborhood` | `WellLocatedNeighborhood ≡ Neighborhood ⊓ ((has-amenity some MetroStation) ⊔ (is-near some (Neighborhood ⊓ (has-amenity some MetroStation))))` | Missing `has-amenity` links to metro prevents well-located inference. |
| `Fasanviertel : WellLocatedNeighborhood` | `is-near` must be symmetric and `Sonnenwendviertel` must already be well-located | Fasanviertel uses the near-a-metro-neighborhood branch of the definition. |
| `Mariahilf/DonauCity : UrbanNeighborhood` | `UrbanNeighborhood ≡ WellLocatedNeighborhood ⊓ (has-amenity min 2 Shop) ⊓ (has-amenity min 2 Restaurant)` | Requires all three conditions: well-located + 2 shops + 2 restaurants. |
| `FamilyNeighborhood` not inferred | `FamilyNeighborhood ≡ ResidentialNeighborhood ⊓ (has-amenity some School) ⊓ (has-amenity some Park)` | Even with school/park individuals present, wrong property (`contains` only) blocks inference. |
| Neighborhood not inferred as `ResidentialNeighborhood` | `ResidentialNeighborhood ≡ Neighborhood ⊓ (contains some ResidentialBuilding) ⊓ (contains only (not Industrial))` plus closure assumptions | Under OWA, "only non-Industrial" usually needs closure axioms to be provable. |
| Desirable apartment missing | `DesirableApartment ≡ Apartment ⊓ (located-in some (WellLocatedNeighborhood ⊓ ResidentialNeighborhood))` and transitive `located-in` | Need apartment->building->neighborhood chain and neighborhood classification. |
| Expected amenity-based inferences missing globally | Bridge `has-amenity ⊑ located-in` or explicit `has-amenity` assertions | Data often entered with `contains/located-in`; definitions may require `has-amenity`. |
