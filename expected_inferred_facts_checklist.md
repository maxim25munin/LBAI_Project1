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
