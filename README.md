**`#![no_std]`** version of crate **[fixedstr](https://crates.io/crates/fixedstr/)**:
library for strings of fixed maximum lengths that can be copied and
stack-allocated using const generics.
<br>

**THIS CRATE WILL NOT BE UPDATED FURTHER.  Instead, version 0.4 of the
[fixedstr](https://crates.io/crates/fixedstr/)** crate now supports
no_std as an option.**


#### Examples
```
  let a:str8 = str8::from("abcdef"); //a str8 can hold up to 7 bytes
  let a2 = a;  // copied, not moved
  let ab = a.substr(1,5);  // copies substring to new string
  assert_eq!(ab, "bcde");  // can compare for equality with &str
  assert_eq!(ab.len(),4);
  println!("str8: {}", &a);   // impls Display
  assert_eq!(&a[..3], "abc"); // impls Index for Range types
  assert!(a<ab); // and Ord, Hash, Debug, Display, Eq, other common traits
  let astr:&str = a.to_str(); // convert to &str (zero copy)
  let azstr:zstr<16> = zstr::from(a); //zstr is a zero-terminated string
  let a32:str32 = a.resize(); // same kind of string but with 31-byte capacity  
  let mut u = str8::from("aλb"); //unicode support
  assert_eq!(u.nth(1), Some('λ'));  // get nth character
  assert_eq!(u.nth_ascii(3), 'b');  // get nth byte as ascii character
  assert!(u.set(1,'μ'));  // changes a character of the same character class
  assert!(!u.set(1,'c')); // .set returns false on failure
  assert!(u.set(2,'c'));
  assert_eq!(u, "aμc");
  assert_eq!(u.len(),4);  // length in bytes
  assert_eq!(u.charlen(),3);  // length in chars
  let mut ac:str16 = a.reallocate().unwrap(); //copies to larger capacity type
  let remainder = ac.push("ghijklmnopq"); //append up to capacity, returns remainder
  assert_eq!(ac.len(),15);
  assert_eq!(remainder, "pq");
  ac.truncate(9);  // keep first 9 chars
  assert_eq!(&ac,"abcdefghi");
  let (upper,lower) = (str8::make("ABC"), str8::make("abc"));
  assert_eq!(upper, lower.to_ascii_upper()); // no owned String needed

  let c1 = str8::from("abcd"); // string concatenation with + for strN types  
  let c2 = str8::from("xyz");
  let c3 = c1 + c2;           
  assert_eq!(c3,"abcdxyz");
  assert_eq!(c3.capacity(),15);  // type of c3 is str16

  let c4 = str_format!(str16,"abc {}{}{}",1,2,3); // impls std::fmt::Write
  assert_eq!(c4,"abc 123");  //str_format! truncates if capacity exceeded
  let c5 = try_format!(str8,"abcdef{}","ghijklmn");
  assert!(c5.is_none());  // try_format! returns None if capacity exceeded
```

Consult the [documentation](https://docs.rs/no_std_strings/latest/no_std_strings/) for details.

