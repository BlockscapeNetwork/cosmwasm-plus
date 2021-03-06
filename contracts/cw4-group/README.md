# CW4 Group

This is a basic implementation of the [cw4 spec](../../packages/cw4/README.md).
It fufills all elements of the spec, including the raw query lookups,
and it designed to be used as a backing storage for 
[cw3 compliant contracts](../../packages/cw3/README.md).

It stores a set of members along with an admin, and allows the admin to
update the state. Raw queries (intended for cross-contract queries) 
can check a given member address and the total weight. Smart queries (designed
for client API) can do the same, and also query the admin address as well as
paginate over all members.

## Init

To create it, you must pass in a list of members, as well as an optional
`admin`, if you wish it to be mutable.

```rust
pub struct InitMsg {
    pub admin: Option<HumanAddr>,
    pub members: Vec<Member>,
}

pub struct Member {
    pub addr: HumanAddr,
    pub weight: u64,
}
```

Members are defined by an address and a weight. This is transformed
and stored under their `CanonicalAddr`, in a format defined in
[cw4 raw queries](../../packages/cw4/README.md#raw).

Note that 0 *is an allowed weight*. This doesn't give any voting rights, but
it does define this address is part of the group. This could be used in
eg. a KYC whitelist to say they are allowed, but cannot participate in
decision-making.

## Messages

Update messages and queries are defined by the 
[cw4 spec](../../packages/cw4/README.md). Please refer to it for more info.